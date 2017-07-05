# 00 代理模式
说到代理模式, 可以和外卖简单类比

在没有外卖出现之前, 我们想买些东西, 自己到指定地点购买(可能我自己比较懒惰, 买完了就回来, 哪里都不去了)

有了外卖之后, 我们一样可以得到自己想要的东西, 只不过是外卖员充当了代理的角色, 利用我们的资源帮助我们完成

外卖员还有其他的送餐任务, 可以先给别人送出, 然后给我送, 最后再给另外的人送餐

## 00.00 简单的代码实现

**模拟自己买东西:**
```csharp
public interface IBuyModule
{
    void BuySomething();
}

public class BuyModule : IBuyModule
{
    public void BuySomething()
    {
        Console.WriteLine("buy food!");
    }
}

staic void Main(String[] args)
{
    IBuyModule buy = new BuyModule();
    buy.BuySomething();
}
```

**创建送餐员代理:**
```csharp
public class BuyModuleProxy : IBuyModule
{
    BuyModule _bm;
    public BuyModuleProxy()
    {
        _bm = new BuyModule();
    }

    public void BuySomething()
    {
        _bm.BuySomething();
    }
}

staic void Main(String[] args)
{
    IBuyModule buyProxy = new BuyModuleProxy();
    buyProxy.BuySomething();
}
```

BuyModuleProxy 充当我们自己和卖东西地点之间的中间人角色, 可以在调用真实方法 BuySomething 之前和之后执行任何想要的操作

```csharp
public void BuySomething(){
    Console.WriteLine("给别人买东西");
    Console.WriteLine("给别人送东西");
    _bm.BuySomething(); // 给我们自己买
    Console.WriteLine("给我们送东西");
}
```

> 对于只有一个方法的类来说还可以接受, 但如果这个类有十几个方法, 我们也还要实现十几个吗!! 显然不会这么做