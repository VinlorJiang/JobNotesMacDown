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
