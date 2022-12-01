---
title: "为何在Java的世界里Spring一家独大"
subtitle: "技术联结了服务和业务"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - java
  - philosophy
---

> Java的世界里，可以没有标准库，却不能没有Spring。

本人接触`Spring`是快毕业那年，参加学校安排的培训。那时候软件行业不像现在这样家喻户晓，可也是生机勃勃。最初的开发三剑客是`SSH(Spring+Struts+Hibernate)`，当时会这个可是相当吃香的。人人网就是用的这套玩意儿，因为它的网页链接后缀都是以`do`结尾。而那时正火的`Struts`开发出来的网站就有这个特征。彼时人人网如日中天，对经常使用它的人来说很有印象。不过对初学Java的人来说，会用就行，至于底层的东西，老师不讲，网上能找到的资料很少。

到了12年左右，`SpringMVC`出世，以其简易且快速上手的的开发模式迅速赢得了开发者的心。而`Struts`因为其设计上的缺陷，缺乏灵活性，被开发者诟病。虽然`Struts2`试图亡羊补牢，不过`SpringMVC`和`Spring`的整合大大的提升了开发效率，`Struts2`缺乏盟友，双拳难敌四手，从此默默淡出人们的视线。很快`Hibernate`被后起新秀`Mybatis`所替代，`SSH`变成了`SSM(Spring+SpringMVC+Mybatis)`。

`Spring`极有野心，其家族遍布软件开发的各个角落(`Spring+SpringBoot+SpringCloud`)。无论是`NoSQL(Redis+MongoDB+Memcache)`，还是消息队列`(RabbitMQ+RocketMQ+ActiveMQ)`，亦或其他的中间件，你总能在它的家族里找到不错的解决方案。你需要花极少的时间学习，写极少的代码，就能在你的项目里添加新的组件。在Java的开发世界里，它成了真正的无冕之王。易学易上手是其特点，开发方式极其傻瓜化。 这也促成了国内培训机构的泛滥，培训3个月或者半年就能参加工作，完成一些基本码农能干的事。

对很多开发者而言，会用`Spring`可谓成也萧何，败也萧何。`Spring`的设计屏蔽了Java底层的不少细节，对工作了几年的开发人员来说，有可能只知`Spring`却忘了Java基础。对大多数以业务为主的公司而言，`Spring`如闺中美人、匣中利剑一般，能快速完成项目，好招人。那么，Spring到底为何在如今的Java世界里一家独大。

本文仅仅讲一些个人的思考，管中窥豹，图的是得到一点思考的乐趣。

### 抄、超、潮

这个观点是我从某位大师身上听来的，`Spring`从`J2EE`和`J2SE`那里抄袭代码设计的思想，重新设计一套简易的接口给开发者使用（超），其设计的接口具有时代前卫性，进而引领潮流。下面举例说明

`Spring`里的资源接口是`Resource`

```java
/**
 * Interface for a resource descriptor that abstracts from the actual
 * type of underlying resource, such as a file or class path resource.
 *
 * <p>An InputStream can be opened for every resource if it exists in
 * physical form, but a URL or File handle can just be returned for
 * certain resources. The actual behavior is implementation-specific.
 *
 * @author Juergen Hoeller
 * @since 28.12.2003
 * @see #getInputStream()
 * @see #getURL()
 * @see #getURI()
 * @see #getFile()
 * @see WritableResource
 * @see ContextResource
 * @see UrlResource
 * @see FileUrlResource
 * @see FileSystemResource
 * @see ClassPathResource
 * @see ByteArrayResource
 * @see InputStreamResource
 */
public interface Resource extends InputStreamSource {

	/**
	 * Determine whether this resource actually exists in physical form.
	 * <p>This method performs a definitive existence check, whereas the
	 * existence of a {@code Resource} handle only guarantees a valid
	 * descriptor handle.
	 */
	boolean exists();

	/**
	 * Indicate whether non-empty contents of this resource can be read via
	 * {@link #getInputStream()}.
	 * <p>Will be {@code true} for typical resource descriptors that exist
	 * since it strictly implies {@link #exists()} semantics as of 5.1.
	 * Note that actual content reading may still fail when attempted.
	 * However, a value of {@code false} is a definitive indication
	 * that the resource content cannot be read.
	 * @see #getInputStream()
	 * @see #exists()
	 */
	default boolean isReadable() {
		return exists();
	}

	/**
	 * Indicate whether this resource represents a handle with an open stream.
	 * If {@code true}, the InputStream cannot be read multiple times,
	 * and must be read and closed to avoid resource leaks.
	 * <p>Will be {@code false} for typical resource descriptors.
	 */
	default boolean isOpen() {
		return false;
	}

	/**
	 * Determine whether this resource represents a file in a file system.
	 * A value of {@code true} strongly suggests (but does not guarantee)
	 * that a {@link #getFile()} call will succeed.
	 * <p>This is conservatively {@code false} by default.
	 * @since 5.0
	 * @see #getFile()
	 */
	default boolean isFile() {
		return false;
	}

	/**
	 * Return a URL handle for this resource.
	 * @throws IOException if the resource cannot be resolved as URL,
	 * i.e. if the resource is not available as descriptor
	 */
	URL getURL() throws IOException;

	/**
	 * Return a URI handle for this resource.
	 * @throws IOException if the resource cannot be resolved as URI,
	 * i.e. if the resource is not available as descriptor
	 * @since 2.5
	 */
	URI getURI() throws IOException;

	/**
	 * Return a File handle for this resource.
	 * @throws java.io.FileNotFoundException if the resource cannot be resolved as
	 * absolute file path, i.e. if the resource is not available in a file system
	 * @throws IOException in case of general resolution/reading failures
	 * @see #getInputStream()
	 */
	File getFile() throws IOException;

	/**
	 * Return a {@link ReadableByteChannel}.
	 * <p>It is expected that each call creates a <i>fresh</i> channel.
	 * <p>The default implementation returns {@link Channels#newChannel(InputStream)}
	 * with the result of {@link #getInputStream()}.
	 * @return the byte channel for the underlying resource (must not be {@code null})
	 * @throws java.io.FileNotFoundException if the underlying resource doesn't exist
	 * @throws IOException if the content channel could not be opened
	 * @since 5.0
	 * @see #getInputStream()
	 */
	default ReadableByteChannel readableChannel() throws IOException {
		return Channels.newChannel(getInputStream());
	}

	/**
	 * Determine the content length for this resource.
	 * @throws IOException if the resource cannot be resolved
	 * (in the file system or as some other known physical resource type)
	 */
	long contentLength() throws IOException;

	/**
	 * Determine the last-modified timestamp for this resource.
	 * @throws IOException if the resource cannot be resolved
	 * (in the file system or as some other known physical resource type)
	 */
	long lastModified() throws IOException;

	/**
	 * Create a resource relative to this resource.
	 * @param relativePath the relative path (relative to this resource)
	 * @return the resource handle for the relative resource
	 * @throws IOException if the relative resource cannot be determined
	 */
	Resource createRelative(String relativePath) throws IOException;

	/**
	 * Determine a filename for this resource, i.e. typically the last
	 * part of the path: for example, "myfile.txt".
	 * <p>Returns {@code null} if this type of resource does not
	 * have a filename.
	 */
	@Nullable
	String getFilename();

	/**
	 * Return a description for this resource,
	 * to be used for error output when working with the resource.
	 * <p>Implementations are also encouraged to return this value
	 * from their {@code toString} method.
	 * @see Object#toString()
	 */
	String getDescription();
}

```

这一个接口就整合了Java里的各种资源，`File`、`URL`、`InputStream`、`URI`。和`URL`相关的`URLConnection`有多种实现，比如`sun.net.www.protocol.ftp.FtpURLConnection`、`sun.net.www.protocol.file.FileURLConnection`。和`File`、`InputStream`相关的在`java.io`包下。而`Spring`把常用的一些给抽象出来，可以看前面代码里的`@ses`注释，`ClassPathResource`、`FileSystemResource`等。

从学习成本上来讲，Java类库涵盖的类比`Spirng`多了不少，接口设计上也是百家争鸣。而`Spring`统一抽象了资源接口，其常见的几个派生类在注释上被罗列出来。相比原生Java类库而言，这极大的简化了开发者需要理解的东西。

通过契约编程的方式，开发者对`Resource`接口的熟练掌握就可应对大多数的开发场景。接口方法上的注释写明了作用，自哪个版本开始出现(`@since`)。学习成本不高，曲线不陡峭，相比Java原生庞杂的类库，这简直太easy了。

从这个例子也能看出，`Spring`的接口设计是面向实际工作场景、面向生产环境的。而原生Java则是大而全，对开发者不友好的。

当大多数人使用`Spring`，其影响力就足以反馈到Java社区，进而在`JSR`、`JEP`中体现。

### 把用户惯成傻子

`Spring`家族里的`SpringBoot`、`SpringCloud`可谓out of box，开箱即用。很多做开发的也知道它的`AutoConfiguration`，`Spring`从4.0开始添加了很多`Condition`注解，由此可以引入`SpringBoot`模块，实现自动装配。无需像从前那样通过`xml`或者`Configuration`类手动配置`Bean`，同时利用`IOC`里`Bean`生命周期的钩子，给用户留下手动配置的方式。

我们不仅需要理解其中的工作原理，更要知道`Spring`这么做是何居心？

一路走来，`Spring`提供的开发方式始终把开发者惯成傻子。开发者却乐此不疲，把`Spring`奉为圭臬。`Spring`不仅表现出技术上的成功，进而延申到服务和商业上。这种模式不是它首创的，微软的操作系统、苹果的产品皆是如此。

把用户惯成傻子，用户便任凭我摆布。我掌握了核心的话语权，也就有了定价权、产品自主权。很多搞创业的讲商业模式上的护城河，你和其他同类最大的区别是什么，如何保住自己的利润。一句话，培养用户的心智，让她（他）对你的产品流连忘返。很多时候一款产品并不是它技术上多么的领先，材质多么的好，而是广告里给用户的心理定位影响多深。一如曾经的Adidas椰子球鞋，被炒到天价。

软件被开发出来是给人用的，好不好用由用户说了算。所以很多时候我们常说，不懂产品的程序员不是好的程序员。

软件开发里的接口可能给自己用，也可能给别人用，把别人当傻子，把自己当Master（The force with be with you!）。我们的接口不仅服务于同事，也传承给后来的维护者。

### 对领域知识的理解决定了成败

`Spring`之所以能设计出如此简易上手的接口，在于它多年的积累，理解了大部分的开发场景需求。这不是一蹴而就的，是冰冻三尺，日积月累之功。软件开发里的领域知识不仅包含技术上各种实现细节，还有时代发展里的各种概念，云计算、容器化、微服务。软件领域知识相当的庞杂，现在这个时代通常合一个公司的力量来理解。

对软件开发者而言，领域知识可能是业务的细节。对产品经理而言，是竞品、市场、用户心理等多个方面。对餐馆的老板而言，是菜品、店铺的位置、厨师的手艺、消费者的喜好、食材的选购。

每个人对于自己所处的位置需要花功夫去理解需要的领域知识。

写代码的程序员往往不懂产品，因而和产品经理闹别扭是常有的事。而产品经理也不懂代码里的逻辑，往往被嘲讽不切实际、云里雾里。

### 结语

后两点是个人从`Spring`的设计推演到现实的商业和生活上，此文仅作笔记。

