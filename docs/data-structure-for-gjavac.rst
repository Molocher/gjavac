数据类型
============

* String: 同Java的String类型,String是否相等只能用==,暂不支持equals()方法,UvmCoreLibs.tostring(Object obj)转换为String.
* Long: 同Java的Long类型,使用UvmCoreLibs.tointeger(Object obj)转换为Long.
* Boolean: 布尔类型,同Java
* UvmMap: UvmMap.create()创建,操作方法同Java的HashMap.
* UvmArray: UvmArray.create()创建,操作方法同Java的ArrayList,元素下标从1开始.
