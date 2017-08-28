---
layout: post
title: ScalaTest---判断页面没有的元素是否存在
tags: ScalaTest Selenium Scala
categories: ScalaTest
---

##### PageModel定义元素位置如下：#####
```
class RewardsActivitiesPage(driver: WebDriver) extends Page(driver) with ExpwebUriBuilder {
    val pageIdValue = "page.rewards.statement"
    val pageURL = ""
    object activityFeed {
        val hotelsLink: WebElement = Try(driver.findElement(By.xpath("//a[@href='/Hotels']"))).getOrElse(null)
        val homepageSearchWizardLink: WebElement = Try(driver.findElement(By.className("to-next-tier"))).getOrElse(null)
    }
}
```

1.一般情况下，判断页面元素是否存在是，我们会直接使用方法isDisplaysed
```
val newRewardsActivityPage = new RewardsActivitiesPage(webDriver).activityFeed
assert(newRewardsActivityPage.hotelsLink.isDisplayed, "ERROR: Next year progress title not found")
```

这个是当页面存在这个元素时，我们可以判断到这个元素是否有显示
而我们经常会遇到一种情况就是，这个元素本来不存在于页面，但是需要判断它不存在于页面上。

eg:当我们使用等级为blue的用户时这个元素不存在，要判断它不存在才是正确的,而当我们使用等级为sliver的用户登录是，这个就会存在。

这时候当我们选择blue用户登录时，页面不存在这个元素，及时我们在PageModel中定义了这个元素，然后用isDisplay去判断是否显示。就会报错，因为在new这个对象时，没有找到该元素就会返回空值。

这时候我们可以写一个方法来实现，如下：
```
//参数 by 传入查找元素的条件
def isElementExisted(driver: WebDriver,by: By):Boolean={
    try{
      driver.findElement(by)
      return true
    }
    catch{case e:NoSuchElementException => return false}
}
```

使用如下：
```

isElementExisted(webDriver,By.className("box user-bar")) shouldBe false
```
## `===============   版权声明：本文为博主原创文章，未经博主允许不得转载   ===============`
