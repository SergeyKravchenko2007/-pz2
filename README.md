С# -pz2

using System;
using System.Collections.Generic;

public abstract class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Code { get; set; }
    public int Quantity { get; set; }

    public Product(string name, decimal price, string code, int quantity)
    {
        Name = name;
        Price = price;
        Code = code;
        Quantity = quantity;
    }

    public abstract void ShowInfo();

    public virtual void Sell(int amount)
    {
        if (amount <= Quantity)
        {
            Quantity -= amount;
            Console.WriteLine($"Продано {amount} шт. товару '{Name}'");
        }
        else
        {
            Console.WriteLine($"Недостатньо товару '{Name}' на складі");
        }
    }
}

public class Food : Product
{
    public DateTime ExpiryDate { get; set; }

    public Food(string name, decimal price, string code, int quantity, DateTime expiryDate)
        : base(name, price, code, quantity)
    {
        ExpiryDate = expiryDate;
    }

    public override void ShowInfo()
    {
        Console.WriteLine($"Їжа: {Name}, Ціна: {Price} грн, Код: {Code}, Кількість: {Quantity}, Термін придатності: {ExpiryDate.ToShortDateString()}");
    }

    public override void Sell(int amount)
    {
        if (DateTime.Now > ExpiryDate)
        {
            Console.WriteLine($"Товар '{Name}' прострочено!");
            return;
        }
        base.Sell(amount);
    }
}

public class Electronics : Product
{
    public int WarrantyMonths { get; set; }

    public Electronics(string name, decimal price, string code, int quantity, int warrantyMonths)
        : base(name, price, code, quantity)
    {
        WarrantyMonths = warrantyMonths;
    }

    public override void ShowInfo()
    {
        Console.WriteLine($"Електроніка: {Name}, Ціна: {Price} грн, Код: {Code}, Кількість: {Quantity}, Гарантія: {WarrantyMonths} міс.");
    }

    public override void Sell(int amount)
    {
        base.Sell(amount);
        if (amount > 0)
        {
            Console.WriteLine($"Гарантія на '{Name}' - {WarrantyMonths} місяців");
        }
    }
}

public class Clothing : Product
{
    public string Size { get; set; }

    public Clothing(string name, decimal price, string code, int quantity, string size)
        : base(name, price, code, quantity)
    {
        Size = size;
    }

    public override void ShowInfo()
    {
        Console.WriteLine($"Одяг: {Name}, Ціна: {Price} грн, Код: {Code}, Кількість: {Quantity}, Розмір: {Size}");
    }
}

class Program
{
    static void Main()
    {
        List<Product> products = new List<Product>
        {
            new Food("Хліб", 25.50m, "F001", 50, DateTime.Now.AddDays(3)),
            new Food("Молоко", 32.00m, "F002", 30, DateTime.Now.AddDays(7)),
            new Electronics("Телефон", 15000.00m, "E001", 10, 24),
            new Electronics("Ноутбук", 25000.00m, "E002", 5, 36),
            new Clothing("Футболка", 450.00m, "C001", 25, "M"),
            new Clothing("Джинси", 1200.00m, "C002", 15, "L")
        };

        Console.WriteLine("Всі товари в магазині:");
        foreach (var product in products)
        {
            product.ShowInfo();
        }

        Console.WriteLine("\nПродаж товарів:");
        products[0].Sell(2);
        products[2].Sell(1);
        products[4].Sell(30);

        Console.WriteLine("\nІнформація після продажів:");
        foreach (var product in products)
        {
            product.ShowInfo();
        }

        Console.WriteLine("\nБазова поведінка товарів:");
        foreach (var product in products)
        {
            product.Sell(1);
        }
    }
}
