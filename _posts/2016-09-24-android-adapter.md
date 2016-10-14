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

#### Basics

We use the adpater class to set up pagination when displaying a list view. The adapter achieves this by sitting between our data and view.

![layout diagram for adapter shows adapter between data and our view](/assets/images/adapter/adapter-layout-diagram.jpg)

When scrolling a list of items from the ListView, any items that you scroll out of are sent to a part of the `ListView` called the Recycler. The Recycler allows the Adapter to take over our now unused view using `convertView()`, and return that view instead of creating a new one.

![diagram shows item being removed from listview and being sent to recycler](/assets/images/adapter/recycler-diagram.jpg)


Lets take a look at what `getView()` would look like to achieve something like this.


````java
public View getView(int position, View convertView, ViewGroup parent) {
  if (convertView == null) {
    convertView = mInflater.inflate(R.layout.item, null);
  }
  ((TextView) convertView.findViewById(R.id.text)).setText(DATA[postion]);
  ((ImageView) convertView.findViewById(R.id.icon)).setImageBitmap((position & 1) == 1 ? mIcon1 : mIcon2);
}
````

Our code takes a view named `convertView`, and if it has not been inflated yet, it will do so in the first code block. This will continue until your current view is full. 

















