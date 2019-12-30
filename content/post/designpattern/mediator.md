---
title: "Mediator Pattern"
authorbox: true
categories:
  - "design-pattern"
tags:
  - "Java"
---


**Mediator Pattern** is one of the behavioral patterns and it aims to decouple some related objects by using a separate object to manage their communications.
This may better be explained by an example.
Think of the stock trading system as a mediator to manage buying and selling of stocks.
Individual investors do not talk to other tradres directly, instead they interact with the trading system directly.


Let's define the mediator interface as below.
```java
public interface Mediator {
    void addTrader(Trader trader);
    void buy(double amount);
    void sell(double amount);
}
```

and the trading system implements the interface:
```java
public class Market implements Mediator {
    private List<Trader> traders;
    private double value;

    public Market() {
        traders = new ArrayList<>();
        value = 10000;
    }

    @Override
    public void addTrader(Trader trader) {
        traders.add(trader);
    }

    @Override
    public void buy(double amount) {
        value -= amount;
    }

    @Override
    public void sell(double amount) {
        value += amount;
    }

    @Override
    public String toString() {
        return "Market{" +
                "value=" + value +
                '}';
    }
}
```

Now we have investors ready to trade in the market:
```java
public class Trader {
    protected Mediator mediator;
    protected double cash;
    protected double stock;

    public Trader(Mediator mediator, double cash) {
        this.mediator = mediator;
        this.cash = cash;
        stock = 0;
    }

    void buy(double amount) {
        if (amount > cash) {
            System.out.println("You don't have that much money!");
            return;
        }
        mediator.buy(amount);
        cash -= amount;
        stock += amount;
    }

    void sell(double amount) {
        if (amount > stock) {
            System.out.println("You don't have that much stock to sell!");
            return;
        }
        mediator.sell(amount);
        cash += amount;
        stock -= amount;
    }

    @Override
    public String toString() {
        return "Trader{" +
                "cash=" + cash +
                ", stock=" + stock +
                '}';
    }
}
```

Let the game begin:
```java
public class TradingSystem {

    public static void main(String[] args) {
        Market market = new Market();
        Trader xiaoMing = new Trader(market, 1000);
        Trader gouDan = new Trader(market, 500);
        Trader sanMao = new Trader(market, 1300);

        market.addTrader(xiaoMing);
        market.addTrader(gouDan);
        market.addTrader(sanMao);

        xiaoMing.buy(200);
        gouDan.buy(300);
        sanMao.buy(500);
        System.out.println(market.toString());
        System.out.println(xiaoMing.toString());
        System.out.println(gouDan.toString());
        System.out.println(sanMao.toString());

        gouDan.sell(100);
        sanMao.sell(200);
        System.out.println();
        System.out.println(market.toString());
        System.out.println(xiaoMing.toString());
        System.out.println(gouDan.toString());
        System.out.println(sanMao.toString());
    }
}
```

Here is the output:
```java
Market{value=9000.0}
Trader{cash=800.0, stock=200.0}
Trader{cash=200.0, stock=300.0}
Trader{cash=800.0, stock=500.0}

Market{value=9300.0}
Trader{cash=800.0, stock=200.0}
Trader{cash=300.0, stock=200.0}
Trader{cash=1000.0, stock=300.0}
```