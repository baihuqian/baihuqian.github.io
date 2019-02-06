---
layout: post
published: true
title: How to Mock in Scala Unit Tests
date: '2019-02-06 14:57'
tags:
  - Scala
---

This post is a short summary of mocking in unit tests. I mainly use ScalaTest as the test framework and Mockito as the mocking library. This is not intended to be comprehensive; rather, it is my two cents on how to write maintainable and rich unit tests.

## What to Mock
When writing unit tests for a piece of code, usually a class, trait, or an object, you may want to mock the following:
1. RPC calls and IO operations that depend on the availability of remote systems, file system structures or network configuration.
1. Complex computations outside your code under test. Such computations can include those with large body of code (and should be tested independently) outside your code under test, those using third-party libraries such as crypto, hashing, etc., or those do not yield deterministic results.
1. Tightly-coupled classes with your code under test whose behavior cannot be deterministically controlled from your test setup.

## How to Wire Mock Objects in your Code
You need to provide a hook somewhere in your code to replace the actual classes with mocks. You should design it in a way that minimizes exposure to your business logic. In most cases, I expose the hook for mocking using object composition or value overriding.

#### Object Composition
By adding a reference to the class with mock in your class under test, you can instantiate your test object by overriding it with mock.


```scala
// src file 1
trait UtilObjectTrait {
  def blah: Int = {
    ...
  }
}

object UtilObject extends UtilObjectTrait

// src file 2
class ClassUnderTest {
  val utilObject: UtilObjectTrait = UtilObjectTrait
}

// test file
class ClassUnderTestTest extends FunSpec with BeforeAndAfterAll {
  val testObj: ClassUnderTest = new ClassUnderTest {
    override val utilObject = mock[UtilObjectTrait]
  }

  override def beforeAll(): Unit = {
    // place your mock here
  }
}
```

By adding access modifiers, it is possible to expose `utilObject` only to the unit test classes in the same Scala namespace.

#### Value Overriding
Sometimes, you construct and maintain your clients and libraries to perform RPC/IO operations in a separate file outside your class under test. They are often exposed as `val`s of the util classes/objects. By providing a hook to override the default values, you can stub them with mocks in your unit test.

```scala
// src file 1
object Clients {
  private lazy val defaultEc2Client: AmazonEC2 = ...
  var overrideEc2Client: Option[AmazonEC2] = None

  def ec2Client: AmazonEC2 = overrideEc2Client.getOrElse(defaultEc2Client)
}

// src file 2
class ClassUnderTest {
  def describeRegions(): List[String] = {
    Clients.ec2Client.describeRegions().getRegion.asScala.map(_.getRegionName).toList
  }
}

// test file
class ClassUnderTestTest extends FunSpec with BeforeAndAfterAll {
  override def beforeAll(): Unit = {
    val ec2 = mock[AmazonEC2]
    Clients.overrideEc2Client = ec2
    when(ec2.describeRegions()).thenReturn(List("us-east-1"))
  }

  describe("ClassUnderTest") {
    it("describeRegions should return region names") {
      new ClassUnderTest().describeRegions() should equal (List("us-east-1"))
    }
  }
}
```

This way moves the hook out from your business logic.

## How to Implement Mocking Behavior
I personally use `when` with `thenAnswer` to describe the behavior of the mock, which supports returning different values based on the input. For example, the following mock produces different VPC Subnets for each VPC, as if the calls are served by actual EC2.

```scala
when(ec2.createSubnet(any[CreateSubnetRequest])).thenAnswer((invocation: InvocationOnMock) => {
  val req = invocation.getArgumentAt(0, classOf[CreateSubnetRequest])
  new CreateSubnetResult().withSubnet(
    new Subnet().withSubnetId(req.getVpcId.replace("vpc", "subnet"))
    .withVpcId(req.getVpcId)
    .withAvailabilityZone(req.getAvailabilityZone)
    .withCidrBlock(req.getCidrBlock)
    .withIpv6CidrBlockAssociationSet(new SubnetIpv6CidrBlockAssociation()
         .withAssociationId("subnet-cidr-assoc-037774805a35011ca")
         .withIpv6CidrBlock(req.getIpv6CidrBlock)
         .withIpv6CidrBlockState(new SubnetCidrBlockState().withState(SubnetCidrBlockStateCode.Associating))
        ).withAvailableIpAddressCount(507)
    .withState(SubnetState.Pending)
    .withAssignIpv6AddressOnCreation(false)
  )
})
```

Of course, `thenReturn` provides an easier syntax of mocking, if arguments of the mocked method are simple primitives (thus allow the use of `Matcher`):

```scala
// You want your mock to return true if the second parameter is 100.
when(mock.isGreaterThan(any(), eq(100))).thenReturn(true)
```

## How to Check the Invocation of Mocked Methods
In many cases, the behavior of your code under test depends on the result of mocked calls, or whether mocked calls are made depends on certain conditions in your code. In addition, if the remote calls or IO operations are mutating, you want to ensure that they are made correctly. Thus, you should verify the behavior of your mock by capturing the arguments passed into your mock using `ArgumentCaptor`.

```scala
val argCaptor = ArgumentCaptor.forClass(classOf[CreateSubnetRequest])
verify(ec2, times(2)).createSubnet(argCaptor.capture())
val reqs = argCaptor.getAllValues().asScala
// assert reqs
```

Note:
* `verify(ec2).createSubnet()` verifies it is called once.
* `never()` is a shortcut of `times(0)`. You don't have to handle a special case if the `times()` is calculated programmably.
