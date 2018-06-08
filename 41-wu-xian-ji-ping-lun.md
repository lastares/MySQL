```sql
DROP TABLE IF EXISTS `syf_comments`;
CREATE TABLE `syf_comments` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键id',
  `oauth_user_id` int(10) unsigned NOT NULL DEFAULT 0 COMMENT '评论用户id 关联oauth_user表的id',
  `type` tinyint(1) unsigned NOT NULL DEFAULT 1 COMMENT '1：文章评论',
  `pid` int(10) unsigned NOT NULL DEFAULT 0 COMMENT '父级id',
  `article_id` int(10) unsigned NOT NULL COMMENT '文章id',
  `content` text COLLATE utf8_unicode_ci NOT NULL COMMENT '内容',
  `comment_ip` varchar(255) COLLATE utf8_unicode_ci NOT NULL DEFAULT '' COMMENT '评论IP',
  `status` tinyint(1) NOT NULL COMMENT '1:已审核 2：未审核',
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  `deleted_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```

#### 评论列表方法

```php
public function getDataByArticleId($article_id)
{
    $map = [
        'comments.article_id' => $article_id,
        'comments.pid' => 0
    ];
    // 关联第三方用户表获取一级评论
    $data = $this
        ->select('comments.*', 'ou.name', 'ou.avatar')
        ->join('oauth_users as ou', 'comments.oauth_user_id', 'ou.id')
        ->where($map)
        ->orderBy('created_at', 'desc')
        ->get()
        ->toArray();
    foreach ($data as $k => $v) {
        $data[$k]['city'] = getAreaByIp($v['comment_ip']);
        $data[$k]['content'] = htmlspecialchars_decode($v['content']);
        // 获取二级评论
        $this->child = [];
        $this->getTree($v);
        $child = $this->child;
        if (!empty($child)) {
            // 按评论时间asc排序
            uasort($child, function ($a, $b) {
                $prev = isset($a['created_at']) ? $a['created_at'] : 0;
                $next = isset($b['created_at']) ? $b['created_at'] : 0;
                if ($prev == $next) return 0;
                return ($prev < $next) ? -1 : 1;
            });
            foreach ($child as $m => $n) {
                // 获取被评论人id
                $replyUserId = $this->where('id', $n['pid'])->pluck('oauth_user_id');
                // 获取被评论人昵称
                $oauthUserMap = [
                    'id' => $replyUserId
                ];
                $child[$m]['reply_name'] = OauthUser::where($oauthUserMap)->value('name');
            }
        }
        $data[$k]['child'] = $child;
    }
    return $data;
}
```

```php
// 递归获取树状结构
public function getTree($data)
{
    $map = [
        'pid' => $data['id']
    ];
    $child = $this
        ->select('comments.*', 'ou.name', 'ou.avatar')
        ->join('oauth_users as ou', 'comments.oauth_user_id', 'ou.id')
        ->where($map)
        ->orderBy('created_at', 'desc')
        ->get()
        ->toArray();

    if (!empty($child)) {
        foreach ($child as $k => $v) {
            $v['content'] = htmlspecialchars_decode($v['content']);
            $this->child[] = $v;
            $this->getTree($v);
        }
    }

}
```



