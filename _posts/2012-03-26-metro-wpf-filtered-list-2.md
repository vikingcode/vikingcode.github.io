---
layout : page
title: WPF List Filtering - Part 2
date: 2012-03-26
tags: "metro mahapps.metro wpf"
---

Following up from the previous, there are other ways we can improve filtering.

###Refreshing a CollectionViewSource
[Matt Hamilton](/metro-wpf-filtered-list#comment-476065702) pointed out that you can just *refresh* a `CollectionViewSource`. While this still sits on the UI and with complex filtering will still block the UI, it *does* make this form of filtering significantly smoother - not as smooth as a `BackgroundWorker` but you'd be hard pressed to tell even on larger data sets.

        private CollectionViewSource cvs;
        private bool isFirst = true;
        private void TxtSearchTextChanged(object sender, System.Windows.Controls.TextChangedEventArgs e)
        {
            if (cvs == null)
                cvs = Resources["Apps"] as CollectionViewSource;

            if (cvs == null)
                return;

            if (!isFirst)
                cvs.View.Refresh();
            else
            {
                isFirst = true;
                cvs.Filter += CvsFilter;
            }
        }
        
Caching the resource lookup and casting and making sure its set with the `bool` convert this so-so filtering into something halfway respectable. Combine it with the delayed binding technique and for many cases this will do.

###Is it ever OK to use a CollectionViewSource?
Yes. 

* Filtering of items coming in (aka a stream) are super easy to do with a `CollectionViewSource`, and the performance hit is minimal since the item blocks the UI when appearing anyway.
* Using the refresh technique with delayed binding makes a `CollectionViewSource` usable in most circumstances.
* Grouping is easiest to do via a `CollectionViewSource`, even if you combine it with alternative filtering mechanisms


###Still to come...
Hopefully I'll get time to throw together an async-ctp example, reactive extension example and to benchmark them all. However, there is no ETA for that.