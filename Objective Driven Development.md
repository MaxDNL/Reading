# Objective Driven Development
This document describes an objective driven approach to development, whereby a developer is expected to identify and define a development objective so that they develop with intention, fully understanding what they are doing and why so that they may better and more fully understand how. 

This approach to development will help engineers better understand and reason about the constructs that they are developing, why they are doing so, what role these constructs play in the larger system, and how to identify short-comings in their skillsets and development workflow so that they may improve their skills and develop with confidence.


## Table Of Contents

* [Overview](#user-content-overview)
* [Identify](#user-content-identify) 
* [Define](#user-content-identify)
* [Implement](#user-content-identify)
* [Validate](#user-content-identify)


## Overview

1. [**Identify**](#user-content-identify) your objective.

2. [**Define**](#user-content-define) your objective using your chosen testing lexicon.

3. [**Implement**](#user-content-implement) to acheive objective using accepted patterns, idioms, and conventions.

4. [**Validate**](#user-content-validate) your objective by confirming that the objective definition defined using your chosen testing lexicon has been satisfied.


## Methodology

### Identify
Before beging to develop it is important to understand what it is we are actually trying to accomplish, as it is important that we develop with intention. To not do so is to fail before we begin. 

The only way to develop with intention is to fully understand our objective. The first step in doing so is to identify what actually is our objective. Often times hints that can lead us to our objective can be found in a business requirement or a User Story. Though, often times, where a business requirement or a User Story will describe expected feature expectations in terms of a business initiaitve or an expected user behavior it will, very likely, give little or no insight into the engineering requirements necessary for us as developers to successfully implement the requested feature.

This is where the onus will be on us as develper to understand the methodologies, paradigms, architecture, patterns, idioms, and conventions within which we are to perform our work, and, with this information, layout a technical plan, albeit informal, for how we will implement the feature. 

It is in this informal planning that we will identify the classes, components, and sub-systems that we need to develop and integrate with to see the requested feature into reality and begin to realize the efforts necessary to do so. It is this very activity that is identifying our objective.

One process for doing this is to:

1. Understand your User Story.
2. Know your givens.
3. Identify what you need.
4. Identify what you have.
5. Identify the gap between what you need and what you have. _**Implementing the technical constructs to close this gap is your objective.**_

#### Example

##### User Story
_"As an Elite Member Customer I can request to have my orders shipped Overnight Express for free."_

##### Givens
_These are truths that we know independent of any user story. They are the "rules of the game" and driven by factors outside of any individual feature development initiative._

* Favor SOLID OO methods.
* Let Test Driven Development methods lead development.
* ES6, HTML5, CSS, React.
* Favor internal library usage: `kernel`, `react-app`.

##### Needs Analysis
_I need:_

* a Customer
* the concept of Elite Member Customer
* an Order
* the concept of Shipping
* the concept of Overnight Express Shipping
* a policy that defines the business policy of allowing Elite Member Customers to get Overnight Express Shipping for free.

##### Haves Analysis
_I have:_

* a `Customer` class
* the concept of Elite Member in the form of a `Role` as a public member of my `Customer` class
* an `Order` class that has a public `Customer` member.
* a `FulfillmentFactory` that can create `Shipping` with type `OvernightExpress`.

##### Gap Analysis
_I still require:_

* a policy that defines the business policy of allowing Elite Member Customers to get Overnight Express Shipping for free.

##### Objective
_**"Implement a policy that defines the business policy of allowing Elite Member Customers to get Overnight Express Shipping for free."**_

	
### Define
After idenfiying your objective, it is important to define your objective using your chosen testing lexicon. That is, you must be able to write appropriate tests such that you will be able to understand that you have successfully met your objective.

Althought, very much simplified, a todo or grocery list can be seen as a similar construct. After realizing that you need bread and butter (Needs Analysis), and that you do not have any (Haves Analysis), you determine that must get this bread and butter (Gap Analysis). Your objectives then will be to get bread and butter. You will then create a list that has an open box to the left of each of these items in turn in which you will make a mark to identify when you have obtained each to notify yourself that you have successfully covered your gap (Objective). 

The tests that you write are exactly this. They, simply, follow different lexical rules than the traditional todo or grocery list. They will follow the lexical rules defined by the testing framework that you have chosen. Furthermore, they will be more explicit in their nature of identifying that you have, in fact, accomplished your objective. Whereas, it has likely become readily obvious to you what bread and butter are and how to identify them such that you know you didn't mistakenly grab a kumquat to fulfill your get bread objective, it will, likely, not be as obvious when you have fulfilled your Overnight Express Shipping policy objecive. Therefore, your "todo list" will have to include identification mechanisms to determine that you have in fact fulfilled your objective.

#### _Example:_

##### Objective Definition
_**"Implement a policy that defines the business policy of allowing Elite Member Customers to get Overnight Express Shipping for free."**_

##### Fulfillment Definition
* An Overnight Express Shipping Policy...
	* _Defined as an instantiable class `FreeOvernightExpressShippingPolicy` that extends the `AbstractSpecification` defined in the `kernel` library._

* ...will be satisfied by an Order whose Customer is an Elite Member...
	* _Idenfied by a `true` boolean value returned from the `isSatsifiedBy` method of the `FreeOvernightExpressShippingPolicy ` when passed an `Order` that meets this criteria_ 

* ...and will _**not**_ be satisfied by an Order whose Customer is not an Elite Member...
	* _Idenfied by a `false` boolean value returned from the `isSatsifiedBy` method of the `FreeOvernightExpressShippingPolicy ` when passed an `Order` that does **not** meet this criteria_	

##### Example Test (ES6/Mocha)

```javascript
import assert from 'assert';
import { describe, it } from 'mocha';
import { Elite } from 'MemberType';
import Customer from 'Customer';
import Order from 'Order';
import FreeOvernightExpressShippingPolicy from 'FreeOvernightExpressShippingPolicy';

describe('FreeOvernightExpressShippingPolicy Test', () => {
	
  it('should instantiate', () => {
    assert.ok(() => new FreeOvernightExpressShippingPolicy();
  });
  
  it('should not be satisfied by', () => {
  	const policy = new FreeOvernightExpressShippingPolicy();
  	const customer = new Customer();
  	const order = new Order(customer);
  	
  	assert.ok(!policy.isSatisfiedBy(order));
  });
  
  it('should be satisfied by', () => {
  	const policy = new FreeOvernightExpressShippingPolicy();
  	const customer = new Customer(Elite);
  	const order = new Order(customer);
  	
  	assert.ok(policy.isSatisfiedBy(order));
  });
	
});

```

### Implement
Once you have defined your objective, you must meet it. Because you have take then time to truely identify and define your objective, meeting it becomes very simple. You will, simply, implement the necessary class(es) to meet your defined objective.


##### Example FreeOvernightExpressShippingPolicy.js (ES6)

```javascript
import { Assert, AbstractSpecification } from 'kernel';
import { Elite } from 'MemberType';

export default class FreeOvernightExpressShippingPolicy extends AbstractSpecification {
  
  isSatisfiedBy(order) {
  	 return  Assert.exists(order) &&
  	         Assert.exists(order.customer) &&
  	         order.customer.memberType === Elite;
  }

}

```

### Validate

Simply, run your tests! 

Because you have done all of your due dilligence ,validation becomes a breeze. Not only is it simple, but it is extrordinarily valuable to the system as it can be used not only to validate successful objective completion this time, but can be used for automated regression testing and always stand as validation that your code always remains in a state of highest quality.


##Conclusion
With this process you identified an objective, defined it with tests, implemented to achieve it your objective, and validated your success. Using this process for each development task will always poise you to successfully develop only those features required and to develop those features required fully and to the highest quality.

Not only this, but along the way you likely realized some short-comings in knowledge of the system, the process, methods, or paradigms, and, maybe, discovered lacking in skillsets necessary to successfully complete the identified objective. This is also a good thing. For now that you have discovered where you may be currently falling short, you can take the necessary steps continually improve.