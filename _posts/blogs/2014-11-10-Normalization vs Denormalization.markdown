---
layout: blog
category: blogs
title: 'DataBase Normalization vs Denormalization: My thoughts in a NoSQL World'
date: '2014-11-10'
author: Kgao
tags: 'Normalization,Denormalization,NoSQL'
---


> This is not the only post you will find on the internet about it, I create a personal post here more as a personal reference.


A NoSQL (often interpreted as Not Only SQL) database provides a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. Motivations for this approach include simplicity of design, horizontal scaling and finer control over availability. The data structure (e.g. key-value, graph, or document) differs from the RDBMS, and therefore some operations are faster in NoSQL and some in RDBMS. 

NoSQL databases are increasingly used in big data and real-time web applications.

This has really been a long debate as to which approach is more performance orientated

First lets review some basics:

#Pros and Cons of a Normalized database design.

Normalized databases fair very well under conditions where the applications are write-intensive and the write-load is more than the read-load. This is because of the following reasons:

{% highlight bash %}

    Normalized tables are usually smaller and have a smaller foot-print because the data is divided vertically among many tables. This allows them to perform better as they are small enough to get fit into the buffer.
    The updates are very fast because the data to be updated is located at a single place and there are no duplicates.
    Similarly the inserts are very fast because the data has to be inserted at a single place and does not have to be duplicated.
    The selects are fast in cases where data has to be fetched from a single table, because normally normalized tables are small enough to get fit into the buffer.
    Because the data is not duplicated so there is less need for heavy duty group by or distinct queries.

{% endhighlight %}

#Pros and cons of denormalized database design.

Denormalized databases fair well under heavy read-load and when the application is read intensive. This is because of the following reasons:

{% highlight bash %}

    The data is present in the same table so there is no need for any joins, hence the selects are very fast.
    A single table with all the required data allows much more efficient index usage. If the columns are indexed properly, then results can be filtered and sorted by utilizing the same index. While in the case of a normalized table, since the data would be spread out in different tables, this would not be possible.

{% endhighlight %}



#Our Practice:

In our media tech team, we have multiple resources like articles, images, slideshows, videos, and also 3rd party resources.

1. Denormalization:
Since our digital content mainly focus on pages now, we denormalize data in articles collection and normallize other resources.

The reason we denormalize articles because we can sever the page faster, without join multiple collections to gather the information, we can get articles feeds instantly.

Denormalization in articles collection makes sense since the client(CMS) will mainly query that resource, and article almost have one page reuiqred data, and it will be fast to filter them in one collection.

This really imporoved the response speed and reduced lots mongo slow queries, in a high concurrency environment, which will boost the page speed in a very optimistic way.


2. Normalization:

To reduce the duplication of data, separate each department's responsibility and provide a faster update channel for internal resource update, we normalize some resources, like images, slideshows, videos...etc.


We also build relationships, like embeded resource in articles, it brings us such benifits:

{% highlight bash %}

    arranging data into logical groupings such that each group describes a small part of the whole
    minimizing the amount of duplicate data stored in a database
    organizing the data such that, when you modify it, you make the change in only one place
    building a database in which you can access and manipulate the data quickly and efficiently without compromising the integrity of the data in storage


{% endhighlight %}

Only thing we need to take care is to let clients know that it's an embedded resource, and include url to PUT if they want to update the embedded.

i.e. If an image is embedded in multiple slideshows, updating the image will update the slideshows that embed it.
i.e. If an imgage is embedded in multiple articles, updating the image -> mongo-connector-> will automatic sync to article db.


#Conclusion:

Most company like us are actually facing a hybrid (SQL plus NoSQL) environment now.
Denormalized schema can greatly improve performance under extreme read-loads but the updates and inserts become complex as the data is duplicate and hence has to be updated/inserted in more than one places. When we have reference or embedded resources or centralized updtes, we may still need normalize data.



