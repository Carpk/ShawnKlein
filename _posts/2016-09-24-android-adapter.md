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















==========================================================================


This works by allowing any list items that are not in the current view


Creating a new view in Android is very expensive. To scroll through a list of 1000 items will greatly impair the performace of our device



#### ListView

To use the Adapater class, we need to know how ListView works. Creating a new view in Android is very expensive. When scrolling a list of items from the ListView, any items that you scroll out of are sent to a part of the `ListView` called the Recycler. The Recycler allows the Adapter to point to our now unused view using `convertView()`, and reused that view instead of creating a brand new one. 


Adapters get the data and and a child view, then pass it to the AdapterView. The AdapaterView will then display the child view along with the data.

### Common Adapters

##### ArrayAdapter

The ArrayAdapter is an adapter backed by an array of objects. We convert an ArrayAdapter item into a String and put it into a TextView. The TextView is then displayed in the AdapterView(ListView).


##### SimpleCursorAdapter

A SimpleCursorAdapter links the data contained in a Cursor to an Adapter View. The SimpleCursorView binds the data to an AdapterView. This 













Using the Adapter class can greatly improve the performace of an app. while the cost is another layer of abstraction and complexity, 


