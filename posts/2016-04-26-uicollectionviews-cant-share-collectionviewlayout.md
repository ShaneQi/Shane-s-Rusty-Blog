---
title: UICollectionViews Can't Share collectionViewLayout
permalink: uicollectionviews-cant-share-collectionviewlayout-16-04-26
date: 2016-04-26 23:57
---

`collectionViewLayout` doesn’t only contains static layout attributes like itemSize, sectionInset, etc. It also contains the dynamic layout info.

Dynamic layout info means `collectionViewLayout` has frame info for every section and cell of the corresponding `UICollectionView`.

For example, let’s say the `UIColllectionView` A has one section and three cells, while `UICollectionView` B has one section and two cells. If they share one `collectionViewLayout`, the app will crash with an exception.

Because when A was loaded, the `collectionViewLayout` will have three layouts for cells at the indexPath (0, 0), (0, 1) and (0, 3). But in B, there only have two cells in (0, 0) and (0, 1). So when `collectionViewLayout` is applied to B, the layout for (0, 3) is applied to a cell which is not existed.



