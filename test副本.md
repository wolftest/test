[TOC]
#4 权限管理接口
##4.1 功能权限接口
###4.1.1 取功能权限列表
- **功能描述：** 取所有系统定义的功能权限列表
- **访问方式：** GET
- **访问URL：**  permission/functions
- **参数列表：**无
- **返回值：** 返回功能权限数组, 格式为：
	```
	[
		{
		"id":"1",
		"code":"功能码",
		"label":"功能名称",
		"desc":"功能描述" 
		},
		…
	]
	```

- **异常代码及说明 ：** 无
- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/permission/functions
  返回值：
	```
	[{
		"id" : "10",
		"code" : "Contact",
		"label" : "联系人档案管理",
		"desc" : "维护和查看联系人信息"
	}, {
		"id" : "9",
		"code" : "Customer",
		"label" : "客户档案管理",
		"desc" : "维护和查看客户信息"
	}, {
		"id" : "7",
		"code" : "DataImport",
		"label" : "导入数据",
		"desc" : "导入业务数据"
	}, {
		"id" : "8",
		"code" : "DataOutput",
		"label" : "导出数据",
		"desc" : "导出业务数据"
	}, {
		"id" : "4",
		"code" : "SYS_DYNAMIC_FIELD",
		"label" : "自定义属性",
		"desc" : "维护业务对象的自定义属性"
	}, {
		"id" : "2",
		"code" : "SYS_FUNCTION",
		"label" : "功能权限管理",
		"desc" : "维护功能权限"
	}, {
		"id" : "6",
		"code" : "SYS_INVITE_USER",
		"label" : "用户邀请",
		"desc" : "邀请用户使用本应用"
	}, {
		"id" : "5",
		"code" : "SYS_MODIFY_PASSWORD",
		"label" : "修改密码",
		"desc" : "修改密码"
	}, {
		"id" : "3",
		"code" : "SYS_SCOPE",
		"label" : "数据权限设置",
		"desc" : "维护数据权限"
	}, {
		"id" : "1",
		"code" : "SYS_USER",
		"label" : "用户管理",
		"desc" : "维护和查看用户信息"
	}
	]

	```

###4.1.2 将功能权限分配给员工
- **功能描述：** 将功能权限分分配给员工，增加员工的功能权限
- **访问方式：** POST
- **访问URL：**  permission/users
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	userids | String | 是 | 员工ID列表，逗号分隔
	funcs | String | 否 | 功能编码列表，以逗号分隔
	
- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.invalid.parameter |  参数不正确

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/permission/users?userids=60003743188&funcs=SYS_USER,Customer
  返回值：无

###4.1.3 将功能权限从员工收回
- **功能描述：** 将功能权限从员工员工收回，减少员工的功能权限
- **访问方式：** DELETE
- **访问URL：**  permission/users
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	userids | String | 是 | 员工ID列表，逗号分隔
	funcs | String | 否 | 功能编码列表，以逗号分隔

- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.invalid.parameter |  参数不正确

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/permission/users?userids=60003743188&funcs=SYS_USER,Customer
  返回值：无

###4.1.4 查询员工已经分配的功能权限
- **功能描述：** 查询当前应用中，已经分配的员工的功能权限
- **访问方式：** GET
- **访问URL：**  permission/users
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	userids | String | 否 | 员工id列表，以逗号分隔。如果不指定，表示所有员工

- **返回值：** 权限列表map,key为员工ID,值为功能权限数组，没有权限为空数组, 格式为：
	```
	{
	"1":["code1","code2",…],
	"2":["code1","code2"],
	…
	}
	```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.invalid.parameter |  参数不正确

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/permission/users
  返回值：
	```
	{
	"1" : ["SYS_USER", "SYS_FUNCTION", "SYS_SCOPE", "SYS_DYNAMIC_FIELD", "SYS_MODIFY_PASSWORD", "SYS_INVITE_USER", "DataImport", "DataOutput", "Customer", "Contact"]
	}
	```

##4.2 数据设置接口
###4.2.1 开启或者关闭app的数据权限
- **功能描述：** 开启或者关闭当前app的数据权限.关闭，表示本应用中所有的对象不再使用数据权限控制。
- **访问方式：** PUT
- **访问URL：**  
	- dataauth/privilege/apps/enable
	- dataauth/privilege/apps/disable

- **参数列表：**无
- **返回值：** 无
- **异常代码及说明 ：** 无
	
###4.2.2 开启或者关闭Entity的数据权限
- **功能描述：** 开启或者关闭Entity的数据权限，关闭后，表示该Entity不再有数据权限控制
- **访问方式：** PUT
- **访问URL：**  
	- dataauth/privilege/entities/{entityName}/enable
	- dataauth/privilege/entities/{entityName}/disable

- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	entityName | String | 是 | 实体对象（EO)名称
	
- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail  |  参数entityName不正确

###4.2.3 开启或者关闭Entity的数据权限(多个Entities)
- **功能描述：** 开启或者关闭多个Entity的数据权限，关闭后，表示这些Entity不再有数据权限控制
- **访问方式：** PUT
- **访问URL：** 
	- dataauth/privilege/entities/enable
	- dataauth/privilege/entities/disable

- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	PUT | JSON Object | 是 |  JSON数组，每个元数为一个实体对象名称
	>提交的JSON数据格式为：
	```
	["eid1","eid2",…]
	```
	
- **返回值：** 如果成功返回true, 失败返回false
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail  |  提交的数据不正确

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/privilege/entities/enable
  提交数据：
  ```
  ["Customer","Contact"]
  ```
  返回值：
	```
	true
	```
	
###4.2.4 开启或者关闭User的数据权限
- **功能描述：** 开启或者关闭User的数据权限，关闭后，表示该user不受数据权限控制，可以访问所有的数据。
- **访问方式：** PUT
- **访问URL：** 
	- dataauth/privilege/users/{uid}/enable
	- dataauth/privilege/users/{uid}/disable

- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	uid | Long | 是 | User的ID
	
- **返回值：** 无
- **异常代码及说明 ：** 无

###4.2.5 开启或者关闭User在Entity上的是下级关系数据权限
- **功能描述：** 开启或者关闭User的上下级数据权限，关闭后，表示该user在该entity上，不具有下级员工的数据权限，看不到下级员工的数据。
- **访问方式：** PUT
- **访问URL：**  
	- dataauth/subordinates/{entityName}/{uid}/enable
	- dataauth/subordinates/{entityName}/{uid}/disable

- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	entityName | String | 是 | 实体对象（EO)名称
	uid | Long| 是 | User的ID

- **返回值：** 无
- **异常代码及说明 ：** 无

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail  |  参数entityName指定的实体没有不正确

###4.2.6 数据权限状态查询: 当前APP的数据权限状态
- **功能描述：** 查询当前app的数据权限开关状态
- **访问方式：** GET
- **访问URL：**  dataauth/privilege/apps
- **参数列表：** 无
- **返回值：** 返回当前的权限开关状态, true和false:
```
true: enable
false: disable
```
- **异常代码及说明 ：** 无

###4.2.7 数据权限状态查询：实体对象(EO)的数据权限状态
- **功能描述：** 查询指定的实体对象的数据权限开关状态
- **访问方式：** GET
- **访问URL：**  dataauth/privilege/entities/{entityName}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	entityName | String | 是 | 实体对象（EO)名称
	
- **返回值：** 返回当前的权限开关状态, true和false:
```
true: enable
false: disable(enttityName不正确也会返回false)
```

- **异常代码及说明 ：** 无

###4.2.8 数据权限状态查询: User的数据权限状态
- **功能描述：** 查询指定的User的数据权限开关状态
- **访问方式：** GET
- **访问URL：**  dataauth/privilege/users/{uid}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	uid | Long| 是 | User的ID

- **返回值：** 返回当前的权限开关状态, true和false:
```
true: enable（当user不存在时，也返回ture)
false: disable
```

- **异常代码及说明 ：** 无

###4.2.9 数据权限状态查询: User与实体对象的数据权限状态
- **功能描述：** 查询指定的User在指定的实体对象(EO)上的数据权限开关状态
- **访问方式：** GET
- **访问URL：**  dataauth/subordinates/query/status
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	entityName | String | 是 | 实体对象（EO)名称
	uid | Long| 是 | User的ID

- **返回值：** 返回当前的权限开关状态, true和false:
```
true: enable （当user不存在时，也返回ture)
false: disable
```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.query.fail  |  参数entityName指定的EO不正确

##4.3 条件规则授权(Assignment)接口
###4.3.1 数据权限分配assignments
- **功能描述：** 为当前的应用创建一个数据权限规则
- **访问方式：** POST
- **访问URL：**  dataauth/assignments/{uid}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	uid | Long | 是 | User的ID（不检查用户是否存在）
	POST | JSON Object | 是 | JSON数据对象
	>POST的数据对象格式为：
	```
	{
	"entityName":"规则应用的实体（EO)名称",
	"privilege":"规则应用权限，取值为SELECT, UPDATE, DELETE, ALL_PRIVILEGE",
	"condition":" entity上的条件字符串(在指定的EO上的HQL条件字符串)"
	}
	```

- **返回值：** 完整的Assignment规则对象，包含ID, 格式为：
	```
	{
	"id" : 规则ID,
	"appId" : "APP ID",
	"entityName" : "实体对象名称",
	"userId" : 用户ID,
	"privilege" : "权限标识",
	"condition" : "条件",
	"isAssignment" : true,
	"enable" : true
	}
	```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.creation.fail  |  POST数据中entityName没有找到或者条件表达式不正确

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/assignments/1
  返回值：
	```
	{
	"id" : 1,
	"appId" : "com.chanapp.cspdemo.testApp",
	"entityName" : "Contact",
	"userId" : 23,
	"privilege" : "ALL_PRIVILEGE",
	"relationshipNames" : null,
	"condition" : "id>0",
	"isAssignment" : true,
	"enable" : true
	}
	```
	
###4.3.2 删除一条数据权限规则
- **功能描述：** 为当前的应用删除一个数据权限规则
- **访问方式：** DELETE 
- **访问URL：**  dataauth/assignments/{assignmentId}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	assignmentId | Long | 是 | 要删除的规则id
	
- **返回值：** 无
- **异常代码及说明 ：** 无

###4.3.3 查询数据权限规则
- **功能描述：** 查询当前定义的所有数据权限规则
- **访问方式：** GET
- **访问URL：**  dataauth/assignments/query/find
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	privilege | Enum | 否 | 权限标识，取值为：SELECT/UPDATE/DELETE/ALL_PRIVIEGE
	entityName | String | 否 | 实体对象（EO)名称
	uid | Long | 否 | User的ID

- **返回值：** 如果没有查询到符合条件的规则，返回null, 否则返回权限规则列表, 格式为：
	```
	[{
	"id" : 规则ID,
	"appId" : "APP ID",
	"entityName" : "实体对象名称",
	"userId" : 用户ID,
	"privilege" : "权限标识",
	"condition" : "条件",
	"isAssignment" : true,
	"enable" : true
	},
	...
	]
	```

- **异常代码及说明 ：** 无
- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/assignments/query/find
  返回值：
	```
	[{
		"id" : 1,
		"appId" : "com.chanapp.cspdemo.testApp",
		"entityName" : "Contact",
		"userId" : 23,
		"privilege" : "ALL_PRIVILEGE",
		"relationshipNames" : null,
		"condition" : "id>0",
		"isAssignment" : true,
		"enable" : true
	}, {
		"id" : 2,
		"appId" : "com.chanapp.cspdemo.testApp",
		"entityName" : "Contact",
		"userId" : 23,
		"privilege" : "ALL_PRIVILEGE",
		"relationshipNames" : null,
		"condition" : "id>0",
		"isAssignment" : true,
		"enable" : true
	}
	]

	```

###4.3.4 查询指定ID数据权限规则
- **功能描述：** 查询定义的指定规则ID所有数据权限规则
- **访问方式：** GET
- **访问URL：** dataauth/assignments/{assignmentId}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	assignmentId| Long | 是 | 数据权限规则ID

- **返回值：** 如果没有查询到符合条件的规则，返回null, 否则返回权限规则, 格式为：
	```
	{
	"id" : 规则ID,
	"appId" : "APP ID",
	"entityName" : "实体对象名称",
	"userId" : 用户ID,
	"privilege" : "权限标识",
	"condition" : "条件",
	"isAssignment" : true,
	"enable" : true
	}
	```

- **异常代码及说明 ：** 无
- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/assignments/1
  返回值：
	```
	{
		"id" : 1,
		"appId" : "com.chanapp.cspdemo.testApp",
		"entityName" : "Contact",
		"userId" : 23,
		"privilege" : "ALL_PRIVILEGE",
		"relationshipNames" : null,
		"condition" : "id>0",
		"isAssignment" : true,
		"enable" : true
	}
	```

##4.4 数据授权(Grant)接口
###4.4.1 创建Grant操作
- **功能描述：** 当前用户将自已有权限的数据对象，授权给其他用户，授权时，需要指明授权的权限（当前用户丢失权限时，不影响已经授权的数据）
- **访问方式：** POST
- **访问URL：**  dataauth/grants
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	POST | JSON Object | 是 | 提交的数据
	>提交的数据格式为：
	```
	{
	  "entityName":"实体对象（EO)名称",
	  "objectId":"数据对象ID ",
	  "granteeId":"要授权的目标用户ID ", 
	  "granterId":"行使授权的用户ID,通常为当前员工的ID,如果当前用户工作在代理模式，则ID为被代理人的ID,如果当前员工为superUser,则可以随便指定用户ID", 
	  "dataAuthPrivilege":"授予的权限,取值为：SELECT, UPDATE, DELETE, ALL_PRIVILEGE"
	}
	```

- **返回值：** 完整的授权结果对象, 格式为：
	```
	{
	"id":授权ID,
	"appId":"APP ID",
	"entityName":"实体对象（EO)名称",
	"userId":要授权的目标用户ID,
	"granter":行使授权的用户ID,通常为当前员工的ID,如果当前用户工作在代理模式，则ID为被代理人的ID,
	"entityObjectId":数据对象ID,
	"privilege":"授予的权限,取值为：SELECT, UPDATE, DELETE, ALL_PRIVILEGE"
	}
	```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.creation.fail  |  创建授权失败
	>创建授权失败可能是下列原因：
	- app已经禁用数据权限
	-  entityName没有找到或者是一个系统EO
	- 提交的数据中有的属性值为null值
	- 指定了privilege=INSERT
	- 当前用户不是管理员，且指定的granterId不是本人ID或者工作在代理模式时，不是被代理人ID

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/grants
  请求数据:
  ```
  {
  "entityName":"Contact",
  "objectId":"1",
  "granteeId":"60004191073", 
  "granterId":"60004191075",
  "dataAuthPrivilege":"SELECT"
  }
  ```
  
  返回值：
	```
	{
	"id" : 4,
	"appId" : "com.chanapp.cspdemo.testApp",
	"entityName" : "Contact",
	"userId" : 60004191073,
	"granter" : 60004191075,
	"entityObjectId" : 1,
	"privilege" : "SELECT"
	}
	```
	
###4.4.2 删除Grant操作
- **功能描述：** 管理员删除授权记录或者当前用户删除自己的授权记录
- **访问方式：** DELETE
- **访问URL：**  dataauth/grants/{grantId}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	grantId | Long | 是 | 授权ID
	
- **返回值：** 无
- **异常代码及说明 ：** 无

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.delete.fail | 删除授权规则失败
	>删除授权规则失败的原因有：
	- app已经禁用数据权限
	- 授权规则没有找到
	- 当前员工没有权限删除授权规则（不是管理员或者不是自己的授权规则）

###4.4.3 收回无效的授权操作
- **功能描述：** 收回无效的数据对象授权操作，即删除员工已经失去了操作权限的授权记录
- **访问方式：** PUT
- **访问URL：**  dataauth/grants/revoke
- **参数列表：**无
- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail |  当前用户没有权限收回时报错

###4.4.4 查询Grant记录
- **功能描述：** 查询指定ID的授权记录
- **访问方式：** GET
- **访问URL：**  dataauth/grants/{grantId}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	grantId | Long | 是 | 授权ID
	
- **返回值：** 授权对象，格式参考创建授权的返回值。如果没有找到记录，返回空内容。

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.query.fail |  查询出异常

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/grants/4
  返回值：
	```
	{
	"id" : 4,
	"appId" : "com.chanapp.cspdemo.testApp",
	"entityName" : "Contact",
	"userId" : 60004191073,
	"granter" : 60004191075,
	"entityObjectId" : 1,
	"privilege" : "SELECT"
	}
	```
	
###4.4.5 查询Grant记录列表
- **功能描述：** 查询有效的Grant记录列表
- **访问方式：** GET
- **访问URL：**  dataauth/grants/query/effective
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	entityName | String | 是 | 实体对象（EO)名称
	objectId | Long | 是 | 数据对象ID
	privilege | String | 是 | 权限,取值为：SELECT, UPDATE, DELETE, ALL_PRIVILEGE"
	granteeId | Long | 是 |  被授权用户ID
	subordinateEnabled | Boolean | 否 | 是否包含下级用户，默认为false

- **返回值：** 授权对象列表, 格式为：
	```
	[{
	"id":授权ID,
	"appId":"APP ID",
	"entityName":"实体对象（EO)名称",
	"userId":要授权的目标用户ID,
	"granter":行使授权的用户ID,通常为当前员工的ID,如果当前用户工作在代理模式，则ID为被代理人的ID,
	"entityObjectId":数据对象ID,
	"privilege":"授予的权限,取值为：SELECT, UPDATE, DELETE, ALL_PRIVILEGE"
	},
	...
	]
	```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.query.fail |  查询出异常

##4.5 权限代理(Delegation)接口
###4.5.1 创建代理操作
- **功能描述：** 当前用户将自已有数据权限，代理给其它用户
- **访问方式：** POST
- **访问URL：**  dataauth/delegations
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	POST | JSON Object | 是 | 提交数据
	>提交数据格式为：
	```
	{
	  "delegatee":"代理给谁的用户Id",
	  "startTimeStamp": "YYYY-MM-DD", 
	  "endTimeStamp":"YYYY-MM-DD"
	}
	```
	
- **返回值：** 返回创建的代理对象, 格式为：
	```
	{
	"id" : 代理记录ID,
	"appId" : "APP ID",
	"userId" : 当前的员工ID,
	"dlgtUserId" : 代理人ID,
	"startTime" : 开始时间,
	"endTime" : 结束时间,
	"enable" : 是否可用,
	"active" : 是否激活
	}
	```

- **异常代码及说明 ：** 
	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.creation.fail  |  创建代理失败
	>创建代理失败可能是下列原因：
	- app已经禁用数据权限
	- 当前用户是superuser,不能代理给别人
	- 代理人delegatee为空或者为当前用户，或者是superuser
	-  结束时间endTimeStamp小于当前时间或者小于开始时间
	- 其它未知异常
	
- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/delegations
  提交数据:
  ```
  {
  "delegatee":"60004191073",
  "startTimeStamp": "2014-10-10", 
  "endTimeStamp":"2016-01-01"
	}
  ```
  返回值：
	```
	{
	"id" : 2,
	"appId" : "com.chanapp.cspdemo.testApp",
	"userId" : 60004191075,
	"dlgtUserId" : 60004191073,
	"startTime" : 1412870400000,
	"endTime" : 1451577600000,
	"enable" : true,
	"active" : false
	}
	```
	
###4.5.2 删除代理操作
- **功能描述：** 删除指定ID的代理记录
- **访问方式：** DELETE
- **访问URL：**  dataauth/delegations/{id}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	delegationId | Long | 是 | 代理记录ID
	
- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.delete.fail  |  创建代理失败
	>删除代理失败可能是下列原因：
	- app已经禁用数据权限
	- 代理记录没有找到
	- 当前用户没有权限删除（不是superuser并且不是被代理人）

###4.5.3 激活/解除代理操作
- **功能描述：** 当前用户激活/解除代理，在代理激活期间，以被代理人的身份（权限）进行业务操作, 解除后以自己的身份（权限）进行业务操作。
- **访问方式：** PUT
- **访问URL：**  
	- 激活：dataauth/delegations/{delegationId}/activate
	- 解除：dataauth/delegations/{delegationId}/deactivate
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	delegationId | Long | 是 | 代理记录ID
	
- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail  |  激活/解除代理失败
	>激活/解除代理失败的原因：
	-  app已经禁用数据权限
	- 代理记录没有找到
	- 当前用户无权限激活(不是superuser并且不是该代理规则的代理人）
	- 代理记录处于不可用（disable）状态
	- 代理记录不在有效期内
	- 代理人已经激活了其它代理（代理人只能有一个代理关系处于激活状态）
	- 被代理人已经被别人代理（被代理人只能有一个代理关系处于激活状态)

###4.5.4 启用/禁用代理操作
- **功能描述：** 设置代理记录的启用和禁用状态状态（enable/disable）
- **访问方式：** PUT
- **访问URL：** 
	- 启用：dataauth/delegations/{delegationId}/enable
	- 禁用：dataauth/delegations/{delegationId}/deactivate
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	delegationId | Long | 是 | 代理记录ID

- **返回值：** 无
- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.update.fail  |   启用/禁用代理失败
	>启用/禁用代理失败的原因：
	-  app已经禁用数据权限
	- 代理记录没有找到
	- 当前用户无权限激活(不是superuser并且不是该代理规则的被代理人）

###4.5.5 查询代理记录
- **功能描述：** 查询指定ID的代理记录
- **访问方式：** GET
- **访问URL：**  dataauth/delegations/{delegationId}
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	delegationId | Long | 是 | 代理记录ID
	
- **返回值：** 代理对象，格式参考创建代理的返回值。如果没有找到记录，返回空内容。

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.query.fail |  查询出异常

- **使用示例：**
  请求：
  http://localhost:8080/cspdemo/testApp/services/1.0/dataauth/delegations/2
  返回值：
	```
	{
	"id" : 2,
	"appId" : "com.chanapp.cspdemo.testApp",
	"userId" : 60004191075,
	"dlgtUserId" : 60004191073,
	"startTime" : 1412870400000,
	"endTime" : 1451577600000,
	"enable" : true,
	"active" : false
	}
	```
	
###4.5.6 查询代理记录列表
- **功能描述：** 查询有效的Grant记录列表
- **访问方式：** GET
- **访问URL：**  dataauth/delegations/query/find
- **参数列表：**
	参数名称 | 参数类型 | 是否必需 | 说明
	--------|:--------:|:---------:|---- 
	delegatee | Long | 否 | 查询结果等于指定的被代理人的ID
	startTime | String | 否 | 查询代理记录开始时间小于等于指定的开始时间，格式为YYYY-mm-dd 
	endTime | String | 否 | 查询代理记录结束时间大于等于指定的开始时间，格式为YYYY-mm-dd 
	enabled | Boolean | 否 |  如果指定查询代理记录的状态是启用或者停用
	activated| Boolean | 否 |  如果指定查询代理记录的状态激活或者解除激活
	
- **返回值：** 如果没有符合条件的记录，返回空值，否则返回代理对象列表, 格式为：
	```
	[{
	"id" : 代理记录ID,
	"appId" : "APP ID",
	"userId" : 当前的员工ID,
	"dlgtUserId" : 代理人ID,
	"startTime" : 开始时间,
	"endTime" : 结束时间,
	"enable" : 是否可用,
	"active" : 是否激活
	},
	...
	]
	```

- **异常代码及说明 ：** 

	异常代码 |  异常说明
	------------|---------- 
	csp.dataauth.assignment.query.fail |  指定的开始时间和结束时间值不正确，或者其它查询出异常
