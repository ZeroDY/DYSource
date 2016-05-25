```objc
//
//  TestViewController.m
//  TestProject
//
//  Created by 周德艺 on 16/5/24.
//  Copyright © 2016年 周德艺. All rights reserved.
//

#import "TestViewController.h"
#import "DYSegmentControlView.h"
#import "DYSegmentContainerlView.h"
#import "Masonry.h"

@interface TestViewController ()

@property(nonatomic,retain)NSArray *titleArr;
@property(nonatomic,retain)NSMutableArray *viewControllerArr;

@property(nonatomic,retain)DYSegmentControlView *selectTools;
@property(nonatomic,retain)DYSegmentContainerlView *contentScrView;

@end

@implementation TestViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    self.view.backgroundColor = [UIColor brownColor];
    self.titleArr = @[@"我的社区1",@"我的圈子1",@"我1",@"测试1"];
    self.viewControllerArr = [NSMutableArray array];
    
    for (int i = 0; i < 4 ; i++) {
        TestViewController2 *vc = [[TestViewController2 alloc]init];
        vc.view.tag = 10*self.view.tag + i;;
        [self addChildViewController:vc];
        [self.viewControllerArr addObject:vc];
    }
    [self createContentVCSrc];
    [self createSelecterToolsSrc];
    self.automaticallyAdjustsScrollViewInsets = NO;//关键
}

- (void)viewWillAppear:(BOOL)animated{
    [self.selectTools mas_makeConstraints:^(MASConstraintMaker *make) {
        make.height.mas_equalTo(40);
        make.left.right.mas_equalTo(self.view);
        make.top.mas_equalTo(self.view.mas_top).offset(0);
    }];
    [self.contentScrView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.mas_equalTo(self.selectTools.mas_bottom);
        make.left.right.bottom.mas_equalTo(self.view);
    }];
}

-(void)createSelecterToolsSrc
{
    __weak typeof(self) weakSelf = self;
    self.selectTools = [[DYSegmentControlView alloc] initWithStyle:DYSementStyleDefault];
    self.selectTools.selectedColor = [UIColor orangeColor];
    [self.selectTools setTitleArr:self.titleArr andBtnBlock:^(UIButton *button) {
        [weakSelf.contentScrView updateVCViewFromIndex:button.tag];
    }];
    [self.view addSubview:self.selectTools];
}

-(void)createContentVCSrc
{
    __weak typeof(self) weakSelf = self;
    self.contentScrView = [[DYSegmentContainerlView alloc]initWithSeleterConditionTitleArr:self.viewControllerArr andBtnBlock:^(int index) {
        [weakSelf.selectTools updateSelecterToolsIndex:index];
    }];
    [self.view addSubview:self.contentScrView];
}
```