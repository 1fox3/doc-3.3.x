---
title: Spider
meta:
  - name: description
    content: EasySwoole-Spider 可以方便用户快速搭建分布式多协程爬虫。
  - name: keywords
    content: swoole|swoole 拓展|swoole 框架|easyswoole|spider|爬虫
---

## 如何实现分布式

使用setMainHost设置某台机器为主机器,使用setQueueType设置为QUEUE_TYPE_REDIS 或自己实现队列

```php
public static function mainServerCreate(EventRegister $register)
{
    // TODO: Implement mainServerCreate() method.
    $config = Config::getInstance()
        ->setStartUrl('xxx') // 爬虫开始地址
        ->setProduct(new ProductTest()) // 设置生产端
        ->setConsume(new ConsumeTest()) // 设置消费端

        // ---------------------------分布式需要关心的两个配置
        ->setMainHost('xxxx.xxxx.xxxx.xxxx') // 分布式时指定某台机器为主机器
        ->setQueueType(Config::QUEUE_TYPE_REDIS) // 分布式时使用queuetype为QUEUE_TYPE_REDIS，或者自己实现队列
        // ----------------------------end

        ->setProductCoroutineNum(1) // 生产端协程数
        ->setConsumeCoroutineNum(1); // 消费端协程数
    Spider::getInstance()
        ->setConfig($config)
        ->attachProcess(ServerManager::getInstance()->getSwooleServer());
}
```