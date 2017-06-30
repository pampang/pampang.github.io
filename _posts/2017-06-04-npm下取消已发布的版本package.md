突然发现自己在npm package上，发布的一个版本居然出bug了！
于是想要马上回退版本。

经过一番折腾，我是这么处理的：

```
// unpublish npm上面的指定版本
npm login
npm unpublish react-native-ali-push@x.x.x

// 将git仓库上面的数据回滚
cd react-native-ali-push/
git log
git reset ***** --hard
git push origin master --force
```

这里，大家需要注意一下：
`npm unpublish`仅仅在发布后的24小时内有效，假如超过了24小时，则需要连续npm官方去取消发布啦。(想结束要趁早

