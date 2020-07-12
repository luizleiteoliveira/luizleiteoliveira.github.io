---
layout: post
title:  "Creating A/B (Split) test API in 2 hours"
summary: Creating A/B (Split) test API in 2 hours
author: Luiz Leite
date: '2020-05-27 14:35:23 +0530'
category: spring-boot
thumbnail: /assets/img/posts/spring-logo.svg
---

## What the hell is A/B or Split test

A/B test are those tests that, one user has a path, and will always follow this path, 
and some other user will be on another flow in the same version. Nowadays we could use these tests for many things like:


 - [x] Split the users (10% will see the green button and rest another color)        _An important thing is, in this scenario, if a user 'X' will always be on the same path_
        
 - [x] Rollout features. When you will release a new feature, you can make a rollout,  progressively,
  and if exists a bug or performance issue we can improve that part before all users have the same problem.
 
Those things are important but, more relevant is how we can collect data about those tests. 
The simplest approach throws a log and gets this info on Kibana with a dashboard.
 
## What we used
 
 - SpringBoot
 - H2 
 - Hibernate 
 - Spring Web
 - JPA
 
For the first part, this is all we need. A good improvement is to add a cache, another
 thing is to use a better database but with H2 you put this on AWS S3 and handle over there.
 
## The project 
 
For persist, the only class needed is the `Feature.kt`, in this class will have all the algorithm.
 
 ```kotlin
import javax.persistence.Entity
import javax.persistence.GeneratedValue
import javax.persistence.Id

@Entity
data class Feature(var name: String = "",
                   var description: String = "",
                   var percentage: Int = 0) {
    fun userAbleUseFeature(ucode: String): Boolean {
        var ucodeNumber = ucode.hashCode()
        var result = (777 * ucodeNumber * this.id) % 100
        return result < this.percentage
    }

    @Id
    @GeneratedValue
    var id: Long = 0
}
```

The method `.entity.Feature#userAbleUseFeature` the algorithm will make the user be on the new feature or not. 
The number `777` is just a constant, is not completely necessary, but it's important to be a multiplying factor with the feature id.

All tests will be in: `com.luizleiteoliveira.abtest.entity.FeatureTest`

```kotlin
    @Test
    fun `user not able to use feature`() {
        val feature = Feature(name = "name", description = "description", percentage = 10)
        feature.id = 10
        val user = User("ucode")
        Assertions.assertEquals(10,  feature.id) // 777 * 10 * 111111138 = 45115764 % 100 = 64
        val userAbleUseFeature = feature.userAbleUseFeature(user.ucode)
        Assertions.assertFalse(userAbleUseFeature)
    }

    @Test
    fun `user able to use feature increase the percentage`() {
        val feature = Feature(name = "name", description = "description", percentage = 65)
        feature.id = 10
        val user = User("ucode")
        Assertions.assertEquals(10,  feature.id) // 777 * 10 * 111111138 = 45115764 % 100 = 64
        val userAbleUseFeature = feature.userAbleUseFeature(user.ucode)
        Assertions.assertTrue(userAbleUseFeature)
    }

    @Test
    fun `user able to use feature`() {
        val feature = Feature(name = "name", description = "description", percentage = 21)
        feature.id = 10
        val user = User("L")
        Assertions.assertEquals(10,  feature.id) // 76 * 10 * 111111138 = 590520 % 100 = 20
        val userAbleUseFeature = feature.userAbleUseFeature(user.ucode)
        Assertions.assertTrue(userAbleUseFeature)
    }
```  

To finish our project the last thing will be the controller from Feature.

```kotlin
import com.luizleiteoliveira.abtest.FeatureRepository
import com.luizleiteoliveira.abtest.entity.Feature
import org.springframework.web.bind.annotation.*

@RestController
@RequestMapping("/features")
class FeatureController(private val featureRepository: FeatureRepository) {

    @GetMapping
    fun getFeatures(): List<Feature> {
        return featureRepository.findAll().toList()
    }

    @PostMapping
    fun createFeature(@RequestBody feature: Feature): Long {
        featureRepository.save(feature)
        return feature.id
    }

    @PutMapping
    fun updateFeature(@RequestBody feature: Feature): Long {
        featureRepository.save(feature)
        return feature.id
    }

    @GetMapping("/{featureId}")
    fun checkUserAbleToUseFeature(@RequestParam("ucode") ucode: String, @PathVariable featureId: Long): Boolean {
        val feature = featureRepository.findById(featureId).get()
        return feature.userAbleUseFeature(ucode)
    }
}
```

The config will be on  `application.properties`

```properties
spring.h2.console.enabled=true
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.datasource.url=jdbc:h2:file:/tmp/demo
```

## Conclusion
 
There are many forms to apply split tests, for those we comment on beginning will work very well, 
but there are many libs and API's to make this. All cases could have a specific thing that could not be the thing you need.

You can check this project on this [link](https://github.com/luizleite-hotmart/ab-test)

## _Want to follow me?_
 
_You can get in contact me on this social media._

    
 GitHub: [luizleite-hotmart](https://github.com/luizleite-hotmart)
    
 Twitter: luizleite_
    
 Twitch: [coffee_and_code](https://www.twitch.tv/coffee_and_code)
    
 Linkedin: [luizleiteoliveira](https://www.linkedin.com/in/luizleiteoliveira/)
    
 dev.to: [luizleite_](https://dev.to/luizleite_)
 