---
layout: post
title: 开发笔记 & 生活随笔
tag: 博客主页
keywords: 
---

##### 不是时间塑造了我，而是我如何塑造我的时间  |  罗树威

```
public class Me {
	private Me() {}
	private static Me instance;
	private static class MeHoler {
		private static Me me = new Me();
	}
	public static Me getInstance() {
		return MeHoler.me;
	}
	public void doing() {
		System.out.println("Hello coder, enjoy your life!");
	}
	public static void main(String[] args) {
		Me.getInstance().doing();
	}
}
```

