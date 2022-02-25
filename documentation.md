<h1 align="center">Pickaxe-SK - Documentation</h1>
<p align="center">Pickaxe-Sk est un skript permettant d'avoir une pioche custom.</p><br />

## 🏹 Plus d'informations ?
Vous souhaitez avoir des informations sur comment télécharger et installer ce code ?
Rendez vous sur cette page : [README.md](https://github.com/MaxouLeKangou/Pickaxe-SK/blob/main/README.md)

## 👨‍💻 options
- **_Cette partie si dessous, va vous permettre de réaliser des modifications de contenu._**
```
options:
    name: "&8× &6&lPickaxe &8×"
    item: iron pickaxe

    xp_emerald: 70
    xp_diamond: 50
    xp_lapis: 40
    xp_redstone: 35
    xp_gold: 30
    xp_iron: 20
    xp_coal: 5

    time_emerald: 110
    time_diamond: 80
    time_lapis: 60
    time_redstone: 40
    time_gold: 35
    time_iron: 20
    time_coal: 10
```
## 👨‍💻 break
- **_Cette partie si dessous, va vous permettre est le mécanisme global de la pioche._**
```
break:
    name of tool = {@name}:
        cancel event
        event-block = emerald ore, diamond ore, lapis ore, redstone ore, gold ore, iron ore or coal ore:
            add event-item to {pickaxe::ore::%uuid of player%::*}
            set {_loc} to event-location
            set {_block} to event-item

            set event-block to bedrock

        event-block = emerald ore:
            set {_xp} to {@xp_emerald}
            set {_t} to {@time_emerald}
        event-block = diamond ore:
            set {_xp} to {@xp_diamond}
            set {_t} to {@time_diamond}
        event-block = lapis ore:
            set {_xp} to {@xp_lapis}
            set {_t} to {@time_lapis}
        event-block = redstone ore:
            set {_xp} to {@xp_redstone}
            set {_t} to {@time_redstone}
        event-block = gold ore:
            set {_xp} to {@xp_gold}
            set {_t} to {@time_gold}
        event-block = iron ore:
            set {_xp} to {@xp_iron}
            set {_t} to {@time_iron}
        event-block = coal ore:
            set {_xp} to {@xp_coal}
            set {_t} to {@time_coal}
        add {_xp} to {pickaxe::xp::%uuid of player%}
        send action bar from "%{@name}% &d+%{_xp}% &5XP" to player

        wait tick
        while {pickaxe::xp::%uuid of player%} >= (1000+(250*{pickaxe::level::%uuid of player%})/1.3):
            set {pickaxe::xp::%uuid of player%} to rounded down ({pickaxe::xp::%uuid of player%}-(1000+(250*{pickaxe::level::%uuid of player%})/1.3))
            add 1 to {pickaxe::level::%uuid of player%}
            send player title {@name} with subtitle "&dvotre pioche passe au niveau &b%{pickaxe::level::%uuid of player%}%" for 3 seconds

        while true:
            block at {_loc} = bedrock:
                {_t} = 0:
                    set block at {_loc} to {_block}
                    stop
                else:
                    remove 1 from {_t}
                wait second
            else:
                stop
```
## 👨‍💻 experience
- **_Cette partie si dessous, va vous permettre de voir le niveau de votre pioche._**
```
right click with iron pickaxe:
    name of tool = {@name}:
        player is sneaking:
            set {_xp} to rounded down (({pickaxe::xp::%uuid of player%}/(1000+(250*{pickaxe::level::%uuid of player%})/1.3))*10)
            set {_bar} to "&a&m"
            loop {_xp} times:
                set {_bar} to "%{_bar}%-"
            set {_bar} to "%{_bar}%&8&m"
            loop (10-{_xp}) times:
                set {_bar} to "%{_bar}%-"
            set {_xp} to convert({pickaxe::xp::%uuid of player%})
            set {_max} to convert(rounded down (1000+(250*{pickaxe::level::%uuid of player%})/1.3))

            send player title {@name} with subtitle "&d%{_xp}% %{_bar}%&r &5%{_max}%" for 3 seconds    
```
## 👨‍💻 ore
- **_Cette partie si dessous, va vous permettre de récupérer vos minerais._**
```
command /ore:
    trigger:
        {pickaxe::ore::%uuid of player%::*} is set:
            loop {pickaxe::ore::%uuid of player%::*}:
                player has space for loop-value:
                    give loop-value to player
                else:
                    drop loop-value at player
            delete {pickaxe::ore::%uuid of player%::*}
            send player title {@name} with subtitle "&aRécupération des minerais effectué !" for 3 seconds
        else:
            send player title {@name} with subtitle "&cOpération infructueuse !" for 3 seconds  
```
