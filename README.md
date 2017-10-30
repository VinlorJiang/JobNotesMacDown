# JobNotesMacDown
取代笔墨记录那些在工作时间的笔记与ideas

## YYText 学习笔记
## 2017.10.27 周五
* 1）当一个 tableView 有多个同样逻辑的cell（常见与example中）时，点击cell的跳转执行代码可参照下面代码：
```
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    NSString *className = self.classNames[indexPath.row];
    Class class = NSClassFromString(className);
    if (class) {
        UIViewController *ctrl = class.new;
        ctrl.title = _titles[indexPath.row];
        [self.navigationController pushViewController:ctrl animated:YES];
    }
    [self.tableView deselectRowAtIndexPath:indexPath animated:YES];
}
```
* 2）Nullability ， WWDC 2015 推出的新特性之一，接口中为了防止写一大堆 nonnull，Foundation 还提供了一对儿宏，包在里面的对象默认加 nonnull 修饰符，只需要把 nullable 的指出来就行，黑话叫 Audited Region（审核区）
```
NS_ASSUME_NONNULL_BEGIN
@interface Sark : NSObject
@property (nonatomic, copy, nullable) NSString *workingCompany;
@property (nonatomic, copy) NSArray *friends;
- (nullable NSString *)gayFriend;
@end
NS_ASSUME_NONNULL_END
```
* 3）static，static关键字声明的变量必须放在implementation外面，或者方法中，如果不为它赋值默认为0或nil，它只在程序开机“初始化一次”。
  * 修饰局部变量

## 2017.10.30 周一
* 1）NSMutableAttributedString文本样式设置以及富文本展示：在项目开发的过程中或多或少都有遇到过在一段文字中某个字符串需要添加下划线、颜色不同于其他文本、背景色、删除线、行间距等等需求。而NSMutableAttributedString可以帮助你实现这些需求：
···
通过以下方法初始化NSMutableAttributedString
- (instancetype)initWithString:(NSString *)str;
···
