# Деплой Смарт контракта Fuel 

- VPS

- Ubuntu 22.04
  
- SSD 2 GB (Если взять меньше то при деплое будет краш )


## Подготовка Сервера

## Обновление пакетов и системы

  
  ```
  apt update 
  ```

![](https://i.imgur.com/FCnCxfi.png)
    
  
  
  ``` 
  apt upgrade
  ```

[![3.png](https://i.postimg.cc/mg7hmJQp/3.png)](https://postimg.cc/mP2bDdp7)


Нажмите `y `

## Установка Git
  
 
  ```
sudo apt-get install git-all
  ```


проверьте версию git

  
   ```
 git version
  ```


## Установка инструментов Rust 

 
  ```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
[![6.png](https://i.postimg.cc/P56fVHC4/6.png)](https://postimg.cc/5XCMjZqY)

Нажмите `1`


## Настройка оболочки


```
source "$HOME/.cargo/env"
 ```

## Обновление Rust

 
  ```
rustup update stable
  ```

 
  ```
rustup default stable
  ```

## Установка инструментов Fuel 


```
curl --proto '=https' --tlsv1.2 -sSf https://install.fuel.network/fuelup-init.sh | sh
```
[![7.png](https://i.postimg.cc/gc5vQ9ZG/7.png)](https://postimg.cc/PNWpvRy7)



Нажмите `y`


## Configure PATH


```
export PATH="$HOME/.fuelup/bin:$PATH"
```


```
source /root/.bashrc
```


## Installs the toolchain


```
fuelup toolchain install beta-4
```


```
fuelup default beta-4
```


Проверьте версию fuelup


```
fuelup --version
```
[![8.png](https://i.postimg.cc/s23zzDLS/8.png)](https://postimg.cc/v42kLMbH)


## Создание папки контракта


```
mkdir fuel-project
```


```
cd fuel-project
```


## Создание контракта


```
forc new counter-contract
```


Вы должны получить следующее

[![9.png](https://i.postimg.cc/nhhzK2HN/9.png)](https://postimg.cc/pp3RRD5B)


Следующий шаг это изменение файла контракта, выберите удобный для вас способ:


- ## 1 Способ

Перейдите по указанному пути  `fuel-project/counter-contract/src/main.sw` и отредактируйте файл контракта с помощью любого графического приложения. Лично я использую **MobaXterm Professional**


- ## 2 Способ
Открываем фаил напрямую из терминала


```
nano counter-contract/src/main.sw
 
```
[![10.png](https://i.postimg.cc/fb00XCCD/10.png)](https://postimg.cc/v1bBwWCN)


полностью удаляем содержимое файла и 
вставляем контракт :


```
contract;
 
storage {
    counter: u64 = 0,
}
 
abi Counter {
    #[storage(read, write)]
    fn increment();
 
    #[storage(read)]
    fn count() -> u64;
}
 
impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter.read()
    }
 
    #[storage(read, write)]
    fn increment() {
        let incremented = storage.counter.read() + 1;
        storage.counter.write(incremented);
    }
}
```


- ## 3 Способ


### Установка VIM 


```
sudo apt install vim
```


Перейдем к файлу


```
vim counter-contract/src/main.sw
```
[![11.png](https://i.postimg.cc/C1gHx9gW/11.png)](https://postimg.cc/d75TWHDB)


- `V` для перехода в визуальный режим

- `i` `insert`,  для переключения vim в режим вставки

удаляем содержимое и вставляем адрес контракта из предыдущего метода или из  **[официального мануала](https://docs.fuel.network/guides/quickstart/building-a-smart-contract/#writing-a-sway-smart-contract)**


После внесения изменений выполните следующие действия:

- Тыкаем `Esc` для выхода из Insert
- Тыкаем `:w` и жмякаем `Enter` чтобы сохранить изменения
- Тыкаем `:q` to close the document


Если вам непонятно управление в режиме Vim, ознакомьтесь с этим **[мануалом](https://www.freecodecamp.org/news/vim-editor-modes-explained/)**


Переходим в папку с контрактом


```
cd counter-contract
```


## Билд контракта


```
forc build
```

результат должен быть таким 
[![12.png](https://i.postimg.cc/9f0QQ8x4/12.png)](https://postimg.cc/jWVb8XYK)


## Настройка кошелька


Импорт существующего кошелька по сид фразе

Вам нужно импортирвать вашу сид фразу из расширения кошелька `Fuel Wallet` . Ели у вас нету кошелька установите расширение [Fuel Wallet](https://chromewebstore.google.com/detail/fuel-wallet/dldjpboieedgcmpkchcjcbijingjcgok?hl=ru&utm_source=ext_sidebar) 



```
forc-wallet import
```


[![13.png](https://i.postimg.cc/GtPn84Hb/13.png)](https://postimg.cc/H8LhFswN)

скопируйте свою seed-фразу, вставьте ее в командную строку и нажмите `Enter` (после вставки фраза останется невидимой в целях безопасности)

задайте желаемый пароль и нажмите `Enter`

Далее введите ваш пароль снова 


Создаём учетную запись кошелька


```
forc wallet account new
```

[![14.png](https://i.postimg.cc/3Nz0Rb4s/14.png)](https://postimg.cc/K3r8pf5f)

Введите пароль вашего кошелька


[![15.png](https://i.postimg.cc/nzgqjLNS/15.png)](https://postimg.cc/mz3PKTL7)

в терминале должен появиться адрес вашего кошеля


## Создание нового кошелька с помощью командной строки


```
forc wallet new
```

Введите желаемый пароль 
Обязательно сохраните seedphrase из кошелька


## Создаём учетную запись кошелька


```
forc wallet account new
```

Получаем токены из крана
для деплоя контракта нам нужно совсем чутка монет, идем сюда **[faucet](https://faucet-beta-4.fuel.network/?address=fuel13hj0rj3r7443ka9dcv7g9mt89v7pdell2my60kdantx6jqq57yuqqsax06)** и вставляем адрес своего кошелька
либо же напрямую из кошеля зоходим в кран  

[![image.png](https://i.postimg.cc/Bvv4DqdM/image.png)](https://postimg.cc/MngCJ8ZQ)

## Деплой контракта


```
forc deploy --testnet
```


[![16.png](https://i.postimg.cc/V6J4JpZg/16.png)](https://postimg.cc/sQR586rG)



Введите повторно пароль


[![17.png](https://i.postimg.cc/K8mNPRsR/17.png)](https://postimg.cc/pp1zxXvH)


После ввода пароля необходимо ввести индекс кошелька 
введите `0` , далее вам нужно подтвердить транзакцию , тыкаем `y`.


Вуаля красавцы ! готово


[![18.png](https://i.postimg.cc/nhwDXr88/18.png)](https://postimg.cc/pmzT72Xk)


чекаем свой контракт в  [Эксплорере](https://fuellabs.github.io/block-explorer-v2/beta-4/)


[![19.png](https://i.postimg.cc/05fr6jmt/19.png)](https://postimg.cc/WtdsC25r)



