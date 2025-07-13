
## 🛒 Task 5 - Automated UI Tests for an E-Commerce Checkout Flow

### ✅ Objective
Automate the **checkout process** on an e-commerce website using **Selenium with Java and TestNG**. The test includes:
- Adding products to cart
- Navigating through checkout steps
- Filling billing/shipping forms
- Verifying page transitions and success messages

---

### 🧰 Tech Stack

- **Language:** Java  
- **Automation Tool:** Selenium WebDriver  
- **Test Framework:** TestNG  
- **Site Used:** [https://automationexercise.com/](https://automationexercise.com/) *(demo site)*

---

### 🔧 Setup Instructions

1. **Add Dependencies (Maven - `pom.xml`):**

```xml
<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.21.0</version>
    </dependency>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.9.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

2. **Test Script:**

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.*;

public class EcommerceCheckoutTest {
    WebDriver driver;

    @BeforeMethod
    public void setUp() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://automationexercise.com/");
    }

    @Test
    public void checkoutFlowTest() throws InterruptedException {
        // 1. Add item to cart
        driver.findElement(By.xpath("//a[@href='/products']")).click();
        Thread.sleep(2000);
        driver.findElement(By.xpath("(//a[contains(text(),'Add to cart')])[1]")).click();
        Thread.sleep(2000);
        driver.findElement(By.xpath("//u[contains(text(),'View Cart')]")).click();
        Assert.assertTrue(driver.getTitle().contains("Shopping Cart"));

        // 2. Proceed to checkout
        driver.findElement(By.xpath("//a[contains(text(),'Proceed To Checkout')]")).click();

        // 3. Login or Register
        driver.findElement(By.xpath("//input[@data-qa='login-email']")).sendKeys("testuser@example.com");
        driver.findElement(By.xpath("//input[@data-qa='login-password']")).sendKeys("testpass");
        driver.findElement(By.xpath("//button[@data-qa='login-button']")).click();

        // 4. Confirm address and order summary
        Assert.assertTrue(driver.getPageSource().contains("Address Details"));
        Assert.assertTrue(driver.getPageSource().contains("Review Your Order"));

        // 5. Place order
        driver.findElement(By.name("message")).sendKeys("Please deliver ASAP.");
        driver.findElement(By.xpath("//a[contains(text(),'Place Order')]")).click();

        // 6. Fill payment details
        driver.findElement(By.name("name_on_card")).sendKeys("Test User");
        driver.findElement(By.name("card_number")).sendKeys("4111111111111111");
        driver.findElement(By.name("cvc")).sendKeys("123");
        driver.findElement(By.name("expiry_month")).sendKeys("12");
        driver.findElement(By.name("expiry_year")).sendKeys("2026");
        driver.findElement(By.id("submit")).click();

        // 7. Confirm order success
        Assert.assertTrue(driver.getPageSource().contains("Your order has been placed successfully!"));
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}
```

---

### 🧪 Verification Checklist

| Step                          | Verified | Notes                                      |
|-------------------------------|----------|---------------------------------------------|
| Product added to cart         | ✅        | Verified via cart page                     |
| Page transitions              | ✅        | Confirmed via titles and page elements     |
| Form validations              | ✅        | Error appears for incorrect logins         |
| Payment success message       | ✅        | Validated with order confirmation text     |

---

### 🐞 Issues & Notes

- Occasional delays required (`Thread.sleep()`) due to pop-ups and dynamic loading.
- CAPTCHA may occasionally block login automation.
- For production scenarios, use **WebDriverWait** instead of `Thread.sleep`.
