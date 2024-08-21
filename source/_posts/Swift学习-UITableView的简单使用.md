---
title: Swift学习-UITableView的简单使用
date: 2016-03-15 18:24:12
tags: Swift
categories: [Swift]
grammar_cjkRuby: true
---

swift学习，tableView的不同写法，使用懒加载方式

```swift
//
//  ViewController.swift
//  Created by luo.h on 16/10/11.
//  Copyright © 2016年 apple. All rights reserved.
//

import UIKit

//Swift 宏定义
let kScreenHeight = UIScreen.mainScreen().bounds.size.height
let kScreenWidth = UIScreen.mainScreen().bounds.size.width
let kCellIdentifier = "CellIdentifier"

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        view.addSubview(tableView);

        //写法二
        view.addSubview(tableView1);
    }

// MARK: UI懒加载--lazyLoad
    // ----------------------------------------
    // 写法二- tableView lazy 加载
    lazy var tableView : UITableView = {
        let temptableView = UITableView(frame: CGRectMake(0, 0,kScreenWidth, kScreenHeight/2-10), style: UITableViewStyle.Grouped)
        temptableView.delegate = self
        temptableView.dataSource = self
        return temptableView
    }()

    // -------------- 写法二--------------------------
    lazy var tableView1: UITableView! = {
        var tableView = UITableView(frame: CGRectMake(0,kScreenHeight/2,kScreenWidth, kScreenHeight/2-10), style: UITableViewStyle.Grouped)
        tableView.delegate = self
        tableView.dataSource = self
        tableView.registerClass(UITableViewCell.self, forCellReuseIdentifier:kCellIdentifier)
        return tableView
    }()


    // 数据源 lazy 加载
    lazy var dataList:[String] = {
        return ["iOS001","iOS002","iOS003","iOS007"];

    }()
}


// MARK:-- UITableView Delegate
extension ViewController : UITableViewDelegate,UITableViewDataSource
{
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return dataList.count
    }

    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {

        if tableView == self.tableView {
            //写法-
            let identifier = "CellIdentifier"
            var cell = tableView.dequeueReusableCellWithIdentifier(identifier)
            if cell == nil {
                cell = UITableViewCell(style: .Default, reuseIdentifier: identifier)
            }
            cell?.textLabel?.text = "tableView*显示行数\(indexPath.row)-\(dataList[indexPath.row])"
            return cell!;

        }else{

            //写法二
            let cell2 = tableView.dequeueReusableCellWithIdentifier(kCellIdentifier, forIndexPath: indexPath)
            cell2.textLabel?.text = "tableView1*显示行数-\(dataList[indexPath.row])"
            return cell2;
        }
    }
}
```


**OC写法**

```objectivec
#import "ViewController.h"


@interface ViewController () <UITableViewDelegate, UITableViewDataSource>

@property (nonatomic, strong) UITableView *tableView;
@property (nonatomic, strong) NSArray *dataSource;

@end

@implementation ViewController

#pragma mark - life cycle
- (void)viewDidLoad {
    [super viewDidLoad];

    [self.view addSubview:self.tableView];
}


#pragma mark - UITableViewDataSource
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return self.dataSource.count;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:kCellIdentifier];
    cell.textLabel.text = self.dataSource[indexPath.row];
    return cell;
}

#pragma mark - UITableViewDelegate
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}


#pragma mark - getters and setters
NSString * const kCellIdentifier = @"kCellIdentifier";
- (UITableView *)tableView
{
    if (_tableView == nil) {
        _tableView = [[UITableView alloc] initWithFrame:CGRectMake(0,20, self.view.bounds.size.width, self.view.bounds.size.height) style:UITableViewStylePlain];
        _tableView.delegate = self;
        _tableView.dataSource = self;
        [_tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:kCellIdentifier];
    }
    return _tableView;
}

- (NSArray *)dataSource
{
    if (_dataSource == nil) {
        _dataSource = @[@"First controller", @"Second controller", @"Third controller"];
    }
    return _dataSource;
}


@end
```