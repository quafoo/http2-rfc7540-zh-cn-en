# SETTINGS Format / SETTINGS帧格式
> The payload of a SETTINGS frame consists of zero or more parameters, each consisting of an unsigned 16-bit setting identifier and an unsigned 32-bit value.
> 
> ``` 
> +-------------------------------+
> |       Identifier (16)         |
> +-------------------------------+-------------------------------+
> |                        Value (32)                             |
> +---------------------------------------------------------------+
> 						Figure 10: Setting Format
> ```

SETTIGNS帧的负载包含零个或者多个参数，每个参数包含一个16bit的无符号设置标识符 (setting identifier) 和一个32bit的无符号值。

``` 
+-------------------------------+
|       Identifier (16)         |
+-------------------------------+-------------------------------+
|                        Value (32)                             |
+---------------------------------------------------------------+
						图 10: Setting帧格式
```