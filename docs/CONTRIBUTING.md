Source open source hosted on Github, welcome to participate in maintenance:[https://github.com/lina-not-linus/loongson_os_customized](https://github.com/lina-not-linus/LFS-BOOK-loongson).

First, in making `fork` to your warehouse, such as `everyone/loongson_os_customized`，then `clone` To local, and set user information.

```sh
$ git clone https://github.com/lina-not-linus/loongson_os_customized.git
$ cd loongson_os_customized
$ git config user.name "yourname"
$ git config user.email "youremail"
```

Update the content and submit it to your warehouse.

```sh
$ git commit -m "add compil content"
$ git push originLoongson master
```

Finally, you can submit a pull request on GitHub.

In addition, it is recommended to periodically update the contents of your warehouse with project warehouse contents.
```sh
$ git remote add originLoongson https://github.com/lina-not-linus/loongson_os_customized.git
$ git push originLoongson master
```

