## Describe the bug

- 我在使用pamac更新软件时遇到报错，报错内容如下：
- Pamac reported errors when I used it to upgrate the software, the error content is as follows: 

![pamac](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pamac_error.png)

```
正在准备...
正在同步软件包数据库...
正在更新 core.db...
正在更新 extra.db...
正在更新 community.db...
正在更新 multilib.db...
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
错误: multilib.db: GPGME 错误：无数据
无效或已损坏的数据库 (PGP 签名)
同步数据库失败
无事可做.
事务成功完成.
```

----
> error: multilib.db: GPGME error: no data
> invalid or corrupted database (PGP signature)
----

- 使用pacman更新系统是也同样报错：
- Same problem with pacman when i upgrate my system : 

![pacman_error](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pacman_error.png)

```
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
:: 正在同步软件包数据库...
 core                  169.6 KiB   508 KiB/s 00:00 [######################] 100%
 extra                1892.8 KiB  7.39 MiB/s 00:00 [######################] 100%
 community               6.6 MiB  9.90 MiB/s 00:01 [######################] 100%
 multilib              174.6 KiB  2.30 MiB/s 00:00 [######################] 100%
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：GPGME 错误：无数据
错误：failed to synchronize all databases (无效或已损坏的数据库 (PGP 签名))
```

----
> error: GPGME error: No data
> error: failed to synchronize all databases (invalid or corrupted database (PGP signature))
----

- 然后我在论坛里看到这篇帖子「[Error: failed to synchronize all databases ...](https://forum.manjaro.org/t/error-failed-to-synchronize-all-databases-invalid-or-corrupted-database-pgp-signature/76320)」，我的情况和帖子里面的一模一样，我以为我找到了解决方案。
- Then I saw this post 「[Error: failed to synchronize all databases ...](https://forum.manjaro.org/t/error-failed-to-synchronize-all-databases-invalid-or-corrupted-database-pgp-signature/76320)」 in the forum, my problem is exactly the same as pjbrunet's and I thought I found the solution .

## To Reproduce

- 根据pjbrunet的描述，我在`var/lib/pacman/sync`该路径下看到了sig文件并把它们删掉，pacman可以正常使用了，但pamac并没有。我再次使用pamac更新软件时，该路径下依然会产生‘.sig’文件，同样的操作在手机热点下结果一样，所以我并不确定是我的网络问题还是pamac的问题。

![d_sigs](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/d_sigs.png)

----

![pacman_work](https://cdn.jsdelivr.net/gh/Keanu-42/Keanu-42.github.io@v1.4/Gnome使用细节/pamac_error/pacman_work.png)

- According to pjbrunet, I saw the '.sig' files in `var/lib/pacman/sync` and deleted them , pacman works fine but pamac  does not. When I used pamac to upgrating the software again, the '.sig'files was still generated in this path. The same operation had the same result with my  mobile's wifi , so I wasn't sure if it was my network or pamac .

## System information

 - OS: Manjaro Gnome 40
 - pamac version: 10.1.3-3
 - Pacman: v6.0.0 - libalpm v13.0.0

## Additional context

- network: campus network
- mirrorlist: global (refresh frequently)

## Solutions

- 这篇帖子「[Error: failed to update multilib ...](https://forum.manjaro.org/t/error-failed-to-update-multilib-invalid-or-corrupted-database-pgp-signature/18008)」基本概括了我的情况，里面提到的解决方法应该是解决了我的问题，虽然还是有点玄学的成分在里面233
- 关于PGP签名更详细的内容请看Arch Wiki的这篇「[pacman / Package signing](https://wiki.archlinux.org/title/Pacman/Package_signing#Troubleshooting)」

----

 > Hi!
 > Try this
 > ```
 > sudo pacman -Sy archlinux-keyring manjaro-keyring
 > sudo pacman-key --populate archlinux manjaro
 > sudo pacman-key --refresh-keys
 > ```
 > If doesn’t work, try this
 > Delete package cache
 > ```
 > pacman -Sc
 > ```
 > Delete as root
 > `/etc/pacman.d/gnupg`
 > Regenerate it with
 >
 > ```
 > sudo pacman-key --init
 > sudo pacman-key --populate archlinux
 > ```
 > > ———— @visone