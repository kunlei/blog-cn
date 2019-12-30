---
title: "Chain of Responsibility"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Chain of Responsibility** is one of the behavioral design patterns popularized by the original GoF book.
It involves a sender object and potentially multiple receiver objects, and the point of this pattern is to decouple the sender from various receivers. 
Without this pattern, a sender would have to keep reference to all possible receivers and also define the logic of how the sender is processed by receivers.


A good example I can think of is the car maintenance service. 
Every once in a while, I would take my car for routine maintenance and after checking in at front desk, I don't need to worry about how the car is serviced in the shop, the only 'object' I, as a customer, interacts with is the front desk.
In this example, a customer is natually decoupled with maintenance mechanics.


Let's first define a Car object:
```java
/**
 * Car object to be serviced
 * @author Kunlei Lian
 */
public class Car {
    private boolean serviced;
    private boolean needAir;
    private boolean needOilChange;
    private boolean needNewBattery;
    private boolean needNewWiper;

    public Car() {
        serviced = false;
        needAir = true;
        needOilChange = true;
        needNewBattery = false;
        needNewWiper = true;
    }

    public boolean isServiced() {
        return serviced;
    }

    public void setServiced(boolean serviced) {
        this.serviced = serviced;
    }

    public boolean needAir() {
        return needAir;
    }

    public boolean needOilChange() {
        return needOilChange;
    }

    public boolean needNewBattery() {
        return needNewBattery;
    }

    public boolean needNewWiper() {
        return needNewWiper;
    }

    @Override
    public String toString() {
        return "Car{" +
                "needAir=" + needAir +
                ", needOilChange=" + needOilChange +
                ", needNewBattery=" + needNewBattery +
                ", needNewWiper=" + needNewWiper +
                '}';
    }
}
```

Next, we define a Service interface to be implemented by all possible receivers of a car.
```java
/**
 * Service interface
 * @author Kunlei Lian
 */
public interface Service {
    void setNext(Service service);
    void process(Car car);
}
```


Then, we define various receivers:
```java
/**
 * Front desk is the only receiver that interacts with a customer
 * @author Kunlei Lian
 */
public class FrontDesk implements Service {
    private Service nextService;

    public FrontDesk() {
        nextService = null;
    }

    @Override
    public void setNext(Service service) {
        nextService = service;
    }

    @Override
    public void process(Car car) {
        if (car.isServiced()) {
            System.out.println("FrontDestk: all necessary services performed, see you next time!");
        } else {
            System.out.println("FrontDesk: new car service request received, checking condition: ");
            System.out.println("\t" + car.toString());
            System.out.println("FrontDesk: sending car to service queue: ");

            nextService.process(car);
        }
    }
}
```


```java
/**
 * TireService class adds air if needed
 * @author Kunlei Lian
 */
public class TireService implements Service {
    private Service nextService;

    public TireService() {
        nextService = null;
    }

    @Override
    public void setNext(Service service) {
        nextService = service;
    }

    @Override
    public void process(Car car) {
        if (car.needAir()) {
            System.out.println("TireService: Adding air to car!");
        }
        car.setServiced(true);
        nextService.process(car);
    }
}
```



```java
/**
 * OilService performs oil change if necessary
 * @author Kunlei Lian
 */
public class OilService implements Service {
    private Service nextService;

    public OilService() {
        nextService = null;
    }

    @Override
    public void setNext(Service service) {
        nextService = service;
    }

    @Override
    public void process(Car car) {
        if (car.needOilChange()) {
            System.out.println("OilService: performing oil change!");
        }
        car.setServiced(true);
        nextService.process(car);
    }
}
```




```java
/**
 * Install new battery if necessary
 * @author Kunlei Lian
 */
public class BatteryService implements Service {
    private Service nextService;

    public BatteryService() {
        nextService = null;
    }

    @Override
    public void setNext(Service service) {
        nextService = service;
    }

    @Override
    public void process(Car car) {
        if (car.needNewBattery()) {
            System.out.println("BatteryService: installing new battery!");
        }
        car.setServiced(true);
        nextService.process(car);
    }
}
```



```java
/**
 * Wiper service
 * @author Kunlei Lian
 */
public class WiperService implements Service {
    private Service nextService;

    public WiperService() {
        nextService = null;
    }

    @Override
    public void setNext(Service service) {
        nextService = service;
    }

    @Override
    public void process(Car car) {
        if (car.needNewWiper()) {
            System.out.println("WiperService: installing new windshield wiper!");
        }
        car.setServiced(true);
        nextService.process(car);
    }
}
```



Last, we run a demo of the system:
```java
/**
 * Car dealership with free services :)
 * @author Kunlei Lian
 */
public class ServiceCenter {

    public static void main(String[] args) {
        Car car = new Car();
        FrontDesk frontDesk = new FrontDesk();
        TireService tireService = new TireService();
        OilService oilService = new OilService();
        BatteryService batteryService = new BatteryService();
        WiperService wiperService = new WiperService();

        // setup chain of responsibility
        frontDesk.setNext(tireService);
        tireService.setNext(oilService);
        oilService.setNext(batteryService);
        batteryService.setNext(wiperService);
        wiperService.setNext(frontDesk);

        // start processing
        frontDesk.process(car);
    }
}
```


Here is the output:
```java
FrontDesk: new car service request received, checking condition: 
	Car{needAir=true, needOilChange=true, needNewBattery=false, needNewWiper=true}
FrontDesk: sending car to service queue: 
TireService: Adding air to car!
OilService: performing oil change!
WiperService: installing new windshield wiper!
FrontDestk: all necessary services performed, see you next time!
```
