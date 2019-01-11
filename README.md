We have retired the initial release of Snowflake and working on open sourcing
the next version based on
[Twitter-server](https://twitter.github.io/twitter-server/), in a form that can
run anywhere without requiring Twitter's own infrastructure services.

The initial version, released in 2010, was based on Apache Thrift and it
predated [Finagle](https://twitter.github.io/finagle/), our building
block for RPC services at Twitter.  The Snowflake we're using internally is a
full rewrite and heavily relies on existing infrastructure at Twitter to run.
We cannot commit to a date but we're doing our best to add necessary features to
make Snowflake fit for many environments outside of Twitter.

Source code is still in the repository and is reachable from
[snowflake-2010](https://github.com/twitter/snowflake/releases/tag/snowflake-2010)
tag.

We won't be accepting pull requests or responding to issues for the retired
release.

雪花算法的原始版本是[scala](https://github.com/twitter/snowflake/releases/tag/snowflake-2010)版，用于生成分布式ID（纯数字，时间顺序）,订单编号等。

自增ID：对于数据敏感场景不宜使用，且不适合于分布式场景。
GUID：采用无意义字符串，数据量增大时造成访问过慢，且不宜排序。

[https://images2018.cnblogs.com/blog/137422/201806/137422-20180601005737392-1981165973.jpg]

算法描述：

最高位是符号位，始终为0，不可用。
41位的时间序列，精确到毫秒级，41位的长度可以使用69年。时间位还有一个很重要的作用是可以根据时间进行排序。
10位的机器标识，10位的长度最多支持部署1024个节点。
12位的计数序列号，序列号即一系列的自增id，可以支持同一节点同一毫秒生成多个ID序号，12位的计数序列号支持每个节点每毫秒产生4096个ID序号。

结论：

理论上生成速率为kw/秒，所以完全满足一般企业级应用， 算法可靠（去重处理在此也是多此一举）；
性能：100W+/秒；

以上内容及部分代码引用自：
[https://www.cnblogs.com/Hollson/p/9116218.html]
[https://blog.csdn.net/u011499747/article/details/78254990]
