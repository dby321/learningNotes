# 更改您在自管理部署中的密码和自定义数据

## Overview

拥有相应特权的用户可更改自己的密码和自定义数据。[`Custom data`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/system-users-collection/#mongodb-data-admin.system.users.customData) 存储着可选的用户信息。

## Considerations

要生成在此过程中使用的强密码，您可以使用 `openssl` 实用工具的 `rand` 命令。例如，使用以下选项发出 `openssl rand` 来创建 48 个伪随机字节长且采用 Base64 编码的字符串：

```
openssl rand -base64 48
```



## 先决条件

要修改自己的密码和自定义数据，您必须具有分别授予在用户数据库上执行 [`changeOwnPassword`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/privilege-actions/#mongodb-authaction-changeOwnPassword) 和 [`changeOwnCustomData`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/privilege-actions/#mongodb-authaction-changeOwnCustomData) [动作](https://www.mongodb.com/zh-cn/docs/v6.0/reference/privilege-actions/#std-label-security-user-actions)的特权。

1

### 以具有管理用户和角色特权的用户身份进行连接。

连接到具有管理用户和角色特权的[`mongod`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/program/mongod/#mongodb-binary-bin.mongod)或[`mongos`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/program/mongos/#mongodb-binary-bin.mongos) ，例如具有[`userAdminAnyDatabase`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/built-in-roles/#mongodb-authrole-userAdminAnyDatabase)角色的用户。 以下过程将使用在[对自管理部署启用访问控制中](https://www.mongodb.com/zh-cn/docs/v6.0/tutorial/enable-authentication/)创建的`myUserAdmin` 。

```
mongosh --port 27017 -u myUserAdmin -p  --authenticationDatabase 'admin'
```

如果您没有为 [`-p`](https://www.mongodb.com/zh-cn/docs/mongodb-shell/reference/options/#std-option-mongosh.--password) 命令行选项指定密码，[`mongosh`](https://www.mongodb.com/zh-cn/docs/mongodb-shell/#mongodb-binary-bin.mongosh) 会提示输入密码。

2

### 创建具有相应特权的角色。

在 `admin` 数据库中，[`create`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/db.createRole/#mongodb-method-db.createRole) 具有 [`changeOwnPassword`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/privilege-actions/#mongodb-authaction-changeOwnPassword) 和 [`changeOwnCustomData`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/privilege-actions/#mongodb-authaction-changeOwnCustomData) 的新角色。

```
use admin
db.createRole(
   { role: "changeOwnPasswordCustomDataRole",
     privileges: [
        { 
          resource: { db: "", collection: ""},
          actions: [ "changeOwnPassword", "changeOwnCustomData" ]
        }
     ],
     roles: []
   }
)
```

3

### 添加具有此角色的用户。

在 `test` 数据库中，[`create`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/db.createUser/#mongodb-method-db.createUser) 具有创建的 `"changeOwnPasswordCustomDataRole"` 角色的新用户。例如，以下操作创建的用户同时具有内置角色 [`readWrite`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/built-in-roles/#mongodb-authrole-readWrite) 和用户创建的 `"changeOwnPasswordCustomDataRole"`。

## 提示

您可以将 [`passwordPrompt()`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/passwordPrompt/#mongodb-method-passwordPrompt) 方法与各种用户身份验证/管理方法/命令结合使用，以提示输入密码，而不是直接在方法/命令调用中指定密码。不过，您仍然可以像使用早期版本的 `mongo` shell 一样直接指定密码。

```
use test
db.createUser(
   {
     user:"user123",
     pwd: passwordPrompt(),  // or cleartext password
     roles:[ "readWrite", { role:"changeOwnPasswordCustomDataRole", db:"admin" } ] 
   }
)
```

要向现有用户授予新角色，请使用 [`db.grantRolesToUser()`。](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/db.grantRolesToUser/#mongodb-method-db.grantRolesToUser)

## 步骤

1

### 使用适当的特权进行连接。

以具有相应特权的用户身份连接到 [`mongod`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/program/mongod/#mongodb-binary-bin.mongod) 或 [`mongos`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/program/mongos/#mongodb-binary-bin.mongos)。

例如，以下操作以在[先决条件](https://www.mongodb.com/zh-cn/docs/v6.0/tutorial/change-own-password-and-custom-data/#std-label-change-own-password-prereq)部分中创建的 `user123` 的身份连接到 MongoDB。

```
mongosh --port 27017 -u user123 --authenticationDatabase 'test' -p
```

如果您没有为 [`-p`](https://www.mongodb.com/zh-cn/docs/mongodb-shell/reference/options/#std-option-mongosh.--password) 命令行选项指定密码，[`mongosh`](https://www.mongodb.com/zh-cn/docs/mongodb-shell/#mongodb-binary-bin.mongosh) 会提示输入密码。

要检查您是否拥有[先决条件](https://www.mongodb.com/zh-cn/docs/v6.0/tutorial/change-own-password-and-custom-data/#std-label-change-own-password-prereq)部分中指定的特权并查看用户信息，请使用带有 `showPrivileges` 选项的 [`usersInfo`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/command/usersInfo/#mongodb-dbcommand-dbcmd.usersInfo) 命令。

2

### 更改您的密码和自定义数据。

使用 [`db.updateUser()`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/db.updateUser/#mongodb-method-db.updateUser) 方法更新密码和自定义数据。

例如，以下操作将用户密码更改为 `KNlZmiaNUp0B` 并将自定义数据更改为 `{ title: "Senior Manager" }`：

## 提示

您可以将 [`passwordPrompt()`](https://www.mongodb.com/zh-cn/docs/v6.0/reference/method/passwordPrompt/#mongodb-method-passwordPrompt) 方法与各种用户身份验证/管理方法/命令结合使用，以提示输入密码，而不是直接在方法/命令调用中指定密码。不过，您仍然可以像使用早期版本的 `mongo` shell 一样直接指定密码。

```
use test
db.updateUser(
   "user123",
   {
      pwd: passwordPrompt(),  // or cleartext password
      customData: { title: "Senior Manager" }
   }
)
```

根据提示输入密码。