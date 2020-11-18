---
title: 开发笔记 & 生活随笔 | 罗树威
tag: 首页
layout: post
---

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

