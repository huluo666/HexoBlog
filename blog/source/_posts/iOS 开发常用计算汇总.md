---
​title: iOS开发常用计算汇总
tags: iOS
categories: iOS
date: 2017-04-13 10:04:51
grammar_cjkRuby: true
---

1、计算UIScrollView页码

```objectivec
#pragma mark - UIScrollView delegat
- (NSInteger)currentPageWithScrollView:(UIScrollView *)scrollView {
    // 得到每页宽度
    CGFloat pageWidth = scrollView.frame.size.width;
    // 根据当前的x坐标和页宽度计算出当前页数
    NSInteger currentPage = floor((scrollView.contentOffset.x - pageWidth/ 2) / pageWidth)+ 1;
    return currentPage;
}

- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
   NSInteger currentPage = [self currentPageWithScrollView:scrollView];
    NSLog(@":%zd",currentPage);
}
```



2、计算列表数据分页

总数条数：`totalRecord`
每页最大条数：`pageSize`

**算法一:**

```objectivec
NSInteger  totalPage = totalRecord % pageSize == 0 ? totalRecord / pageSize : totalRecord / pageSize + 1 ;
```

**算法二：**

```objectivec
NSInteger  totalPage = (totalRecord + pageSize -1) / pageSize;
//其中 pageSize - 1 就是 totalRecord / pageSize 的最大的余数
```