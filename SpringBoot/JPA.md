## JPA
Jpa (Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象/关联映射工具来管理 Java 应用中的关系数据。它的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。

值得注意的是，Jpa是在充分吸收了现有 Hibernate，TopLink，JDO 等 ORM 框架的基础上发展而来的，具有易于使用，伸缩性强等优点。从目前的开发社区的反应上看，Jpa 受到了极大的支持和赞扬，其中就包括了 Spring 与 EJB3. 0的开发团队。
>注意:Jpa 是一套规范，不是一套产品，那么像 Hibernate,TopLink,JDO 他们是一套产品，如果说这些产品实现了这个 Jpa 规范，那么我们就可以叫他们为 Jpa 的实现产品。

### Spring Boot Jpa
Spring Boot Jpa 是 Spring 基于 ORM 框架、Jpa 规范的基础上封装的一套 Jpa 应用框架，可使开发者用极简的代码即可实现对数据的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展！学习并使用 Spring Data Jpa 可以极大提高开发效率！
>Spring Boot Jpa 让我们解脱了 DAO 层的操作，基本上所有 CRUD 都可以依赖于它来实现

#### Spring Boot Jpa 默认预先生成了一些基本的CURD的方法
例如：增、删、改等等

##### 继承 JpaRepository
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```
##### 使用默认方法
```java
@Test
public void testBaseQuery() throws Exception {
	User user=new User();
	userRepository.findAll();
	userRepository.findOne(1l);
	userRepository.save(user);
	userRepository.delete(user);
	userRepository.count();
	userRepository.exists(1l);
	// ...
}
```

#### 自定义SQL查询
其实 Spring Data 觉大部分的 SQL 都可以根据方法名定义的方式来实现，但是由于某些原因我们想使用自定义的 SQL 来查询，Spring Data 也是完美支持的；在 SQL 的查询方法上面使用@Query注解，如涉及到删除和修改在需要加上@Modifying.也可以根据需要添加 @Transactional对事物的支持，查询超时的设置等。
```java
@Modifying
@Query("update User u set u.userName = ?1 where u.id = ?2")
int modifyByIdAndUserId(String  userName, Long id);
	
@Transactional
@Modifying
@Query("delete from User where id = ?1")
void deleteByUserId(Long id);
  
@Transactional(timeout = 10)
@Query("select u from User u where u.emailAddress = ?1")
User findByEmailAddress(String emailAddress);
```

#### 多表查询
多表查询 Spring Boot Jpa 中有两种实现方式，第一种是利用 Hibernate 的级联查询来实现，第二种是创建一个结果集的接口来接收连表查询后的结果，这里主要第二种方式。

首先需要定义一个结果集的接口类。
```java
public interface HotelSummary {

	City getCity();

	String getName();

	Double getAverageRating();

	default Integer getAverageRatingRounded() {
		return getAverageRating() == null ? null : (int) Math.round(getAverageRating());
	}

}
```
查询的方法返回类型设置为新创建的接口
```java
@Query("select h.city as city, h.name as name, avg(r.rating) as averageRating "
		- "from Hotel h left outer join h.reviews r where h.city = ?1 group by h")
Page<HotelSummary> findByCity(City city, Pageable pageable);

@Query("select h.name as name, avg(r.rating) as averageRating "
		- "from Hotel h left outer join h.reviews r  group by h")
Page<HotelSummary> findByCity(Pageable pageable);
```
使用
```java
Page<HotelSummary> hotels = this.hotelRepository.findByCity(new PageRequest(0, 10, Direction.ASC, "name"));
for(HotelSummary summay:hotels){
		System.out.println("Name" +summay.getName());
}
```
>在运行中 Spring 会给接口（HotelSummary）自动生产一个代理类来接收返回的结果，代码汇总使用 getXX的形式来获取

#### 多数据源的支持
##### 同源数据库的多源支持
日常项目中因为使用的分布式开发模式，不同的服务有不同的数据源，常常需要在一个项目中使用多个数据源，因此需要配置 Spring Boot Jpa 对多数据源的使用，一般分一下为三步：

1 配置多数据源  
2 不同源的实体类放入不同包路径  
3 声明不同的包路径下使用不同的数据源、事务支持  
#### 异构数据库多源支持
比如我们的项目中，即需要对 mysql 的支持，也需要对 Mongodb 的查询等。

实体类声明@Entity 关系型数据库支持类型、声明@Document 为 Mongodb 支持类型，不同的数据源使用不同的实体就可以了
```java
interface PersonRepository extends Repository<Person, Long> {
 …
}

@Entity
public class Person {
  …
}

interface UserRepository extends Repository<User, Long> {
 …
}

@Document
public class User {
  …
}
```
但是，如果 User 用户既使用 Mysql 也使用 Mongodb 呢，也可以做混合使用
```java
interface JpaPersonRepository extends Repository<Person, Long> {
 …
}

interface MongoDBPersonRepository extends Repository<Person, Long> {
 …
}

@Entity
@Document
public class Person {
  …
}
```
也可以通过对不同的包路径进行声明，比如 A 包路径下使用 mysql,B 包路径下使用 MongoDB
```java
@EnableJpaRepositories(basePackages = "com.neo.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.neo.repositories.mongo")
interface Configuration { }
```
