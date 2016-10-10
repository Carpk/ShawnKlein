---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: Android Adapter Class
date:   2016-09-24 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---   

The adapter class can add another layer of abstraction and complexity, but when used correctly, it can greatly improve the performance of your app.

For us to understand the Adapter class, we need to know how the ListView class works.

#### ListView

To use the Adapater class, we need to know how ListView works. Creating a new view in Android is very expensive. When scrolling a list of items from the ListView, any items that you scroll out of are sent to a part of the `ListView` called the Recycler. The Recycler allows the Adapter to point to our now unused view using `convertView()`. and reused that view instead of creating a brand new one. 

Our code should look somthing like the example below. 

````java
public View getView(int position, View convertView, ViewGroup parent) {
  if (convertView == null) {
    convertView = mInflater.inflate(R.layout.item, null);
  }
  ((TextView) convertView.findViewById(R.id.text
}
````

Our code takes a converted view, if it has not been inflated yet, it will do so





























Using the Adapter class can greatly improve the performace of an app. while the cost is another layer of abstraction and complexity, 


