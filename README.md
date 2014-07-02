TB6612FNG Hardwarepins Library
==============================

This library provides an interface to the TB6612FNG dual motor driver.

* [Product information and data sheet](http://www.pololu.com/product/713)

Getting Started
---------------

This library can be imported into Kinoma Studio and added to an application's build path settings. You can then use the TB6612FNG module to create a motor group and motor objects to control up to two DC motors.

The following objects can be created using the TB6612FNG module in this library:

* [Motor](#motor)
* [MotorGroup](#motorgroup)

Example
-------

The following example demonstrates how to instantiate a new Motor object and control the speed and direction of the motor:

```javascript
// get a reference to the TB6612FNG module
var TB6612FNG = require( "TB6612FNG" );

// create a new Motor instance and specify it's two
// digital direction pins and the pwm speed pin
var motor = new TB6612FNG.Motor( 9, 10, 34 );

// initialize the Motor
motor.init();

// set the motor direction and speed
motor.setDirection( TB6612FNG.FORWARD );
motor.setSpeed( 0.5 );
```

The TB6612FNG can control two motors, and with multiple drivers it is possible to control more than that. When controlling multiple motors it is a good idea instantiate a MotorGroup object and add your motor instances to the group. When an individual motor is added to a group, it's functions are delayed until the group's sync method is called, causing all of the motors in the group to be set at the same time.

The following example demonstrates how to create a group of Motor objects that will be synchronized together.

```javascript
// create a new instance of a MotorGroup object - pin 15 is specified
// as the digital pin number used to control the standby mode
var group = new TB6612FNG.MotorGroup( 15 );

// create two motors and add them to the group object - the first two
// parameters in the constructor specify the two digital output pins
// used to control the motor's direction. The third parameter specifies
// the pwm pin used to control the motor's speed.
var m1 = new TB6612FNG.Motor( 9, 10, 34 );
group.add( m1 );

// create motor two and add it to the group
var m2 = new TB6612FNG.Motor( 11, 12, 30 );
group.add( m2 );

// initialize the motor group 
group.init();

// set motor 1 direction and speed
m1.setDirection( TB6612FNG.FORWARD );
m1.setSpeed( 0.5 );

// set motor 2 direction and speed
m2.setDirection( TB6612FNG.BACKWARD );
m2.setSpeed( 0.5 );

// sync the motors in the group
group.sync();
```

API Reference
-------------

The following reference describes the object interfaces defined in the TB6612FNG module.

Motor
-----

The Motor object interface is used to create an object instance that will control a single DC motor connected to the TB6612FNG. The motor object is typically added to a MotorGroup, so that it can be synchronized with additional motors. When setting the speed and direction of a motor that is part of a group, its function will be delayed until the group's sync method is called.

If you are using a Motor object without a MotorGroup, it may be necessary to set the TB6612FNG standby pin to high. The MotorGroup does this by default when you initialize the MotorGroup object by specifying the standby pin, but a Motor object used on its own does not change the standby pin state.

Creating a new instance of a Motor object:

```javascript
var motor = new TB6612FNG.Motor( d1, d2, pwm, config );
```

> The Motor object requires two digital output pins to control the direction of the motor, and one pwm output pin to control the speed.

Initializing the Motor object instance:

```javascript
motor.init( callback );
```

To set the motor's direction, or stop the motor:

```javascript
motor.setDirection( direction, callback );
```

>The direction parameter must be one of the following constants defined in the TB6612FNG module:
>
>* FORWARD
>* BACKWARD
>* BRAKE

To set the motor's speed:

```javascript
motor.setSpeed( speed, callback );
```

>The motor's speed is specified as a normalized value between 0 and 1.

MotorGroup
----------

The MotorGroup object interface can be used to synchronize a group of Motor objects. The MotorGroup also controls the standby pin on the TB6612FNG, and it is good practice to always use a MotorGroup even if you are only controlling a single motor.

Creating a new instance of an MotorGroup object:

```javascript
var group = new TB6612FNG.MotorGroup( standby );
```

>The standby parameter in the constructor specifies the digital output pin used to put the TB6612FNG in and out of standby mode.

Adding a motor object to the group:

```javascript
group.add( motor );
```

Initializing the group (calls init method on all motor objects that have been added to the group):

```javascript
group.init();
```

Synchronize the group of motors by setting the direction and speed of each motor after  having been modified in the corresponding Motor object:

```javascript
group.sync();
```
