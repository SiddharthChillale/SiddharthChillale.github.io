---
title: "Behavioural Design Patterns in C++"
date: 2023-01-24T11:46:00-05:00
draft: false
showToc: true
---
Design patterns in C++ are ways to structure code so as to maximise code reusability and versatility. Below are some of the keypoints of the different behavioural design patterns in C++ condensed into quick bites from a course on LinkedIn Learning. LALALA
### Chain of Responsibility

for series of nested handlers

kinda like the middleware in express js
    
``` cpp 
class Handler{
	virtual Handler* setNext (Handler* nextHandler) = 0;
	virtual Command* handle(Command* cmd)=0;
};
```
    
### Command
    
To reduce coupling between classes that call one another.

Introduce a third middle class that reduces this coupling. e.g. A button class does not need to have a handle to the canvas. It can have a handle to the command class which in turn has a handle to the canvas it wants to run the command in.

1. ClearCanvasCommand  ---> Command Interface (Inheritance)
2. Button 
			members
			=>[ Command* cmd] // takes a command type in constructor
			=>[ onclick() calls cmd->execute()]
     
```cpp
class Command{
	virtual void execute() = 0;
};

class ClearCanvasCommand : Command{
	Canvas* canvas;
	void execute() override{
		canvas->clearAll();	
	}
};

class AddShapeCommand : Command{
	Canvas* canvas;
	Shape* shape;
	// <--constructor-->
	void execute() override {
		canvas->addShape(shape);
	}
	
};

class Button{
	Command* command;
	// <--constructor--> 
	virtual void click() {
		command->execute();
	}
};   
```
    
### Mediator
    
Reduce coupling between complex object dependencies.

Allow an instance of mediator to facilitate communication between different objects. e.g. UI object has TextBox, CheckBox, ButtonElement
    
```cpp
Mediator{
virtual void mediate();
};

UserInterface : Mediator{
tb = new TextBox;
cb = new CheckBox;
bu = new ButtonElement;

virtual void mediate(string eventstring) override{
	switch(eventstring){
		case textbox: tb->doSomething();
		case checkbox: cb->doItsThing();
		case buttonelement: bu->click();
}
};
```
    
### Observer
    
When we want only the objects that care about certain events to be notified of the change in occurence. This is a publisher and subscriber implementation.

```cpp
class Subscriber{
	virtual void notify(Publisher p, message m);
};

class Publisher {
	virtual void subscribe(Subscriber s);
	virtual void unsubscribe(Subscriber s);
	virtual void publish(message m);
};

class ChatGroup : Publisher{
vector<Subscribers> scp;
// when a subscriber subscribes, add it to the vector scp;
// when a subscriber unsubs, remove it from the vector scp;
// in publish, call the notify() function of all subscribers in the vector scp
	void publish() override {
		for(auto& sub : scp){
			sub->notify();
		}
	};
};

class ChatUser : Subscriber{
	void notify() override {} ; // subscriber needs to implement this
};

```
    
### Interpreter
    
When program needs the ability to interface with some sort of language outside its normal comprehension. e.g. understanding and executing mathematical expressions, understanding human languages like eng/ spanish or roman numerals.

```cpp
class Expression {
	virtual void evaluate() = 0;
};

class OperationExpression : Expression{
	std::string op;
	Expression* lhs;
	Expression* rhs;
	int evaluate(){
		switch(op):
			case "+" : return lhs->evaluate() + rhs->evaluate();
			case "-" : return lhs->evaluate() - rhs->evaluate();
			case "*" : return lhs->evaluate() * rhs->evaluate();
			case "/" : return lhs->evaluate() / rhs->evaluate();
			case default: return 0;
	}
};

class NumberExpression : Expression{
	std::string op;
	void evaluate(){
		return std::stoi(op);
	}
};
```
    
### State

when a class can be in different state and needs to change behavior according to this state.

```cpp
class State {
	State* next = nullptr;
	State(State* nextState) : next(nextState){};

	virtual State* getNext() {
		return next;
	}

	// can be any function that operates differently based
	// on which state the object is in. for illustration, 
	// getDescription is used
	virtual string getDescription() = 0; 
};

class PurchasedState : State {
	PurchasedState(State* nextState): State(nextState){}
	string getDescription() override{
		return "Purchased";
	}
};

class InTransitState : State {
	InTransitState(State* nextState): State(nextState){}
	string getDescription() override{
		return "InTransit";
	}

};

class DeliveredState : State {
	DeliveredState (State* nextState): State(nextState){}
	string getDescription() override{
		return "Delivered";
	}

};

class Order{
	State* s;
	Order (State* curState) : s(curState){};
	void goToNext(){
		s = s->getNext();
	}
	string getCurStateDescription{
		if(!s) return "No states";
		return s->getDescription();	
	}
};

void main(){
	DeliveredState* ds = new DeliveredState(nullptr);
	InTransitState* is = new InTransitState(ds);
	PurchasedState* ps = new PurchasedState(is);
	Order o(ps);
	o->getDescription(); // returns "Purchased"
	
	o->gotToNext();
	o->getDescription(); // returns "InTransit"

	o->gotToNext();
	o->getDescription(); // returns "Delivered"

	o->gotToNext();
	o->getDescription(); // returns "No states"
}
```
    
### Strategy
    
when program needs to choose between several different ways of doing things on the fly.

just like state pattern, but no linking of different strategies to inter-convert them.
    
### Template Method
    
Break entire process into a series of steps, represent each step as a method, and have each different process implement its own steps with their particular variations. 

```cpp
GreetingCardTemplate{
	virtual std::string heading(std::string& to){
		return "Hello" + to;
	}
	virtual std::string ocassion(){
		return "Thank you blah blah";
	}
	virtual std::string footer(std::string& from){
		return "Warm regards, " + from
	}
	std::string generate(std::string& to, std::string& from){
		return heading(to) + ocassion() + footer(from);
	}
};

BirthdayCardTemplate : GreetingCardTemplate{
	std::string ocassion() override{
		return "Happy Birthday";
	}
};

DiwaliCardTemplate : GreetingCardTemplate{
	std::string ocassion() override{
		return "Shubh Dipawali !!";
	}
	
	virtual std::string footer(std::string& from){
		return "From entire family, 🪔 " + from
	}
};

void main(){
	DiwaliCardTemplate dct; 
	BirthdayCardTemplate bct; 
	
	string diwaliLetter = dct.generate("Raju Chacha", "Ambani");
	string bdayLetter = bct.generate("Ranchoddas Chanchad", "Rastogi");
	string bdayLetter = bct.generate("Farhan", "Rastogi");
}
```
    
### Visitor
    
Add similar extraneous functionality to different classes.

```cpp
class Visitor{
	virtual	void handleA(string name){};
	virtual void handleB(datetime time){};
};

class DatabaseVisitor : Visitor{
	void handleA(string name){
		// save to a database
	};
	void handleB(string action){
		// save to a database
	};
};

class FileSysVisitor : Visitor{
	void handleA(string name, int phone){
		// save to a filesystem
	}
	void handleB(datetime timestamp, string action){
		// save to filesystem
	}
};
class A{
	// Some members
	string name;
	int phone;
	
	void accept(Visitor& v);
};

class B{
	// Some members
	string action;
	datetime time;

	void accept(Visitor& v);
};

void main(){
	B bObj(action, currentTime);
	A aObj("Rohan", 9879879879);
	
	DatabaseVisitor dbv;
	FileSystemVisitor fsv;

	aObj.accept(dbv);   // uses dbv to save aObj to database
	aObj.accept(fsv);   // uses fsv to save aObj to filesystem

	//same for bObj
	bObj.accept(dbv);
	bObj.accept(fsv);
	
}

```
    
### Iterator Pattern

A way to travel through some container. This implements a next() function which enables  the iteration of elements in the container.
    
### Memento

allow some objects to save themselves in such a way that they can’t be modified until they say so. Maintain a vector of const class objects. imagine history of objects, push a const copy of every valid state of object in a vector/ stack. When undo is clicked, clear the current object and fill it with the latest copy from the vector/ stack.
    
```cpp
class Canvas; 

class CanvasMemento {
	friend class Canvas;
	const vector<Shape> shapes;
	CanvasMemento(vector<Shape> shapes) : shapes(shapes) {};
};

class Canvas {
	vector<Shape> shapes;
	vector<CanvasMemento*> oldStates;
	Canvas(){};
	void addShapes(Shape new_shape){
		oldStates.push_back(new CanvasMemento(shapes));
		shapes.add(new_shape);
	};
	void undo(){
		shapes = oldStates.back()->shapes;
		cm.shapes.pop_back();
	};
};
```
    
### Null-Object

Instead of using raw null pointers to inititalize member variables, use a default object to initialise it to. 
    
```cpp
class Node{
	int val;
	virtual	int getVal(){
		return val;
	};
};

class ListNode : Node {
	Node* next = nullptr;
	int getNextVal(){
		return next->getVal(); 
		// this will throw null object error, 
		//if next is not assigned any object
	};
};

///////////////////////

class NullNode : Node{
	int getNextVal() {return 0;}
};

class ListNode : Node {
	Node* next = new NullNode;
	int getNextVal(){
		return next->getVal(); 
		// this will not throw error. 
		// The uninitialized object has val set to 0
	};
};
```
    


💡 **Composition vs Inheritance:** 
Difference between having a parent class and having the same class as a member variable:  Having a parent class means that you can handle the child class’s parent members by casting the handle of the child class to a pointer of parent class. Having an object of the parent class as a member doesn’t grant you this capability.

