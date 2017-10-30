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
```
通过以下方法初始化NSMutableAttributedString
- (instancetype)initWithString:(NSString *)str;
```
```
通过以下代码可以添加所需要的样式
- (void)addAttribute:(NSAttributedStringKey)name value:(id)value range:(NSRange)range;
通过以下代码可以移除已添加的样式
- (void)removeAttribute:(NSAttributedStringKey)name range:(NSRange)range;
```
都可设置哪些样式
```
NSFontAttributeName                设置字体属性，默认值：字体：Helvetica(Neue) 字号：12  
NSForegroundColorAttributeNam      设置字体颜色，取值为 UIColor对象，默认值为黑色  
NSBackgroundColorAttributeName     设置字体所在区域背景颜色，取值为 UIColor对象，默认值为nil, 透明色  
NSLigatureAttributeName            设置连体属性，取值为NSNumber 对象(整数)，0 表示没有连体字符，1 表示使用默认的连体字符  
NSKernAttributeName                设定字符间距，取值为 NSNumber 对象（整数），正值间距加宽，负值间距变窄  
NSStrikethroughStyleAttributeName  设置删除线，取值为 NSNumber 对象（整数）  
NSStrikethroughColorAttributeName  设置删除线颜色，取值为 UIColor 对象，默认值为黑色  
NSUnderlineStyleAttributeName      设置下划线，取值为 NSNumber 对象（整数），枚举常量 NSUnderlineStyle中的值，与删除线类似  
NSUnderlineColorAttributeName      设置下划线颜色，取值为 UIColor 对象，默认值为黑色  
NSStrokeWidthAttributeName         设置笔画宽度，取值为 NSNumber 对象（整数），负值填充效果，正值中空效果  
NSStrokeColorAttributeName         填充部分颜色，不是字体颜色，取值为 UIColor 对象  
NSShadowAttributeName              设置阴影属性，取值为 NSShadow 对象  
NSTextEffectAttributeName          设置文本特殊效果，取值为 NSString 对象，目前只有图版印刷效果可用：  
NSBaselineOffsetAttributeName      设置基线偏移值，取值为 NSNumber （float）,正值上偏，负值下偏  
NSObliquenessAttributeName         设置字形倾斜度，取值为 NSNumber （float）,正值右倾，负值左倾  
NSExpansionAttributeName           设置文本横向拉伸属性，取值为 NSNumber （float）,正值横向拉伸文本，负值横向压缩文本  
NSWritingDirectionAttributeName    设置文字书写方向，从左向右书写或者从右向左书写  
NSVerticalGlyphFormAttributeName   设置文字排版方向，取值为 NSNumber 对象(整数)，0 表示横排文本，1 表示竖排文本  
NSLinkAttributeName                设置链接属性，点击后调用浏览器打开指定URL地址  
NSAttachmentAttributeName          设置文本附件,取值为NSTextAttachment对象,常用于文字图片混排  
NSParagraphStyleAttributeName      设置文本段落排版格式，取值为 NSParagraphStyle 对象  
```
代码展示：
```
NSRange range = NSMakeRange(0, title.length);
    NSMutableAttributedString *attribtStr = [[NSMutableAttributedString alloc]initWithString:title];
    //设置下划线
    [attribtStr addAttribute:NSUnderlineStyleAttributeName value:[NSNumber numberWithInteger:NSUnderlineStyleSingle] range:range];
    //字体颜色
    [attribtStr addAttribute:NSForegroundColorAttributeName value:[UIColor blueColor] range:range];
    label.attributedText = attribtStr;
```
```
不一定要用UILabel展示，UITextView和UITextField都可以展示
```
如果需要展示的字符串是带有HTML标签的富文本也可以同NSMutableAttributedString进行展示，代码如下：
```
NSString *path = [[NSBundle mainBundle] pathForResource:@"html" ofType:@"txt"];
NSString *content = [[NSString alloc] initWithContentsOfFile:path encoding:NSUTF8StringEncoding error:nil];
    //修改html中图片的宽度
content = [NSString stringWithFormat:@"<head><style>img{width:%f !important;height:auto}</style></head>%@",CGRectGetWidth(self.view.frame),content];
    //设置富文本
NSMutableAttributedString *attrStr = [[NSMutableAttributedString alloc] initWithData:[content dataUsingEncoding:NSUnicodeStringEncoding] options:@{ NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType } documentAttributes:nil error:nil];
self.textView = [[UITextView alloc]initWithFrame:CGRectMake(0, 0, CGRectGetWidth(self.view.frame) - 5, CGRectGetHeight(self.view.frame))];
[self.textView setBackgroundColor:[UIColor whiteColor]];
self.textView.editable = NO;
self.textView.attributedText = attrStr;
[self.view addSubview:self.textView];
```
