---
title: iOS 重用机制
date: 2016-04-20 15:50:27
tags: iOS
grammar_cjkRuby: true
---



Cell重用

```objectivec
@property (weak, nonatomic) IBOutlet UITableView *myTableView;
- (void)viewDidLoad {
    [super viewDidLoad];

    // Setup table view.
    self.myTableView.delegate = self;
    self.myTableView.dataSource = self;
    [self.myTableView registerClass:[MyTableViewCell class] forCellReuseIdentifier:@"MyTableViewCell"];
}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    static NSString *CellIdentifier = nil;
    UITableViewCell *cell = nil;
    CellIdentifier = @"MyTableViewCell";
    cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier forIndexPath:indexPath];

    cell.textLabel.text = @"Title";

    if (indexPath.row % 2 == 0) {
        [cell.textLabel setTextColor:[UIColor redColor]];
    }
    else {
        [cell.textLabel setTextColor:[UIColor blackColor]];
    }

    return cell;
}
```

总之在复用的时候需要记住：

- **设置 Cell 的存在差异性的那些属性（包括样式和内容）时，有了 if 最好就要有 else，要显式的覆盖所有可能性。**
- **设置 Cell 的存在差异性的那些属性时，代码要放在初始化代码块的外部。**





tableView的组头/组尾重用

```objectivec
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    static NSString *headerReuseIdentifier = @"TableViewSectionHeaderViewIdentifier";
   // Reuse the instance that was created in viewDidLoad, or make a new one if not enough.
    UITableViewHeaderFooterView *sectionHeaderView = [tableView dequeueReusableHeaderFooterViewWithIdentifier:headerReuseIdentifier];
    sectionHeaderView.textLabel.text = @"Non subclassed header";
    return sectionHeaderView;
}
```



`自定义HeaderFooterView`

- 继承UITableViewHeaderFooterView，因为系统提供的UITableViewHeaderFooterView类有重用机制

  ```objectivec
   - (instancetype)initWithReuseIdentifier:(NSString *)reuseIdentifier
  {
      if (self = [super initWithReuseIdentifier:reuseIdentifier]) {
          self.contentView.backgroundColor = XMGCommonBgColor;

          // label
          UILabel *label = [[UILabel alloc] init];
          self.label = label;
      }
      return self;
  }
  ```



- 注册自定义组头视图

  ```
   static NSString * const JPHeaderId = @"header";
     [self.tableView registerClass:[JPCommentHeaderView class] forHeaderFooterViewReuseIdentifier:JPHeaderId];
  ```


- 实现代理方法，返回组头视图

```
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
{
    JPCommentHeaderView *header = [tableView dequeueReusableHeaderFooterViewWithIdentifier:JPHeaderId
                                           ];
    // 覆盖文字
    if (section == 0 && self.hotComments.count) {
        header.text = @"最热评论";
    } else {
        header.text = @"最新评论";
    }
    return header;
}
```





 [iOS UITableViewCell重用问题](http://blog.csdn.net/crayondeng/article/details/8906981)

iOS 高性能异构滚动视图构建方案 —— LazyScrollView

http://pingguohe.net/2016/01/31/lazyscroll.html



http://stackoverflow.com/questions/20188049/uitableview-headerviewforsection-returns-null



[How to implement UIScrollView with 1000+ subviews?](http://stackoverflow.com/questions/2079225/how-to-implement-uiscrollview-with-1000-subviews)



[pagination and reusing views in scrollview](http://stackoverflow.com/questions/13056549/pagination-and-reusing-views-in-scrollview)

[How to reuse UIView like UITableViewCell](http://stackoverflow.com/questions/12545097/how-to-reuse-uiview-like-uitableviewcell)



[UIScrollView极限优化:两个UIImageView循环利用](http://www.wugaojun.com/blog/2015/06/06/uiscrollviewji-xian-you-hua-liang-ge-imageviewxun-huan-li-yong/)

https://github.com/iSevenDays/RecyclingScrollView

https://github.com/malcommac/DMLazyScrollView