```sh
npm info "eslint-config-airbnb-base@latest" peerDependencies  // 可以检查前置依赖

npx install-peerdeps --dev eslint-config-airbnb-base  // 会先在全局临时目录安装 install-peerdeps，然后自动安装前置依赖

// 或者 全局安装 install-peerdeps

npm install -g install-peerdeps
install-peerdeps --dev eslint-config-airbnb-base
```