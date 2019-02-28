---
layout: post
date: 2019-02-25
title: a start job is running by dev/disk 
categories: linux
---

C'est une partie du message que l'on peut lire au démarrage d'une session unix/linux où l'identifiant de la swap a été perdu, si vous n'utilisez pas LVM sur l'ensemble du disque. Il faut alors attendre deux minutes complètes jusqu'à la connexion, et ça semble toujours très long.

Il n'existe pas d'option pour ne pas formater la swap, et donc ne pas lui attribuer de nouvel UUID, dans une installation de Debian et dérivées.

Il existe une méthode relativement simple sous Debian pour retrouver, renommer et réajuster l'identifiant de la partition avant recompilation d'initramfs-tools. Tout se fait depuis le terminal et éventuellement, votre éditeur de texte.

Dans un premier temps, il faut trouver le nouvel UUID de votre swap. Entrez `sudo blkid` et lisez ce que retourne le terminal, jusqu'à apercevoir votre partition de swap.

{% highlight bash %}
/dev/sda6: UUID="47ebbb16-aa14-41df-a003-b483141bb5f7" TYPE="swap" PARTUUID="ed73b1e0-06"
{% endhighlight %}

C'est la nouvelle UUID qui nous intéresse. Sélectionnez le contenu des guillemets et copiez-le.

Depuis le terminal, entrez `sudo geany /etc/fstab` en remplaçant `geany` par votre éditeur de texte. 

Dans les dernières lignes, remplacez l'ancien UUID par le nouveau, afin d'avoir quelque chose comme :

{% highlight bash %}
# swap was on /dev/sda6 during installation
UUID=47ebbb16-aa14-41df-a003-b483141bb5f7 none            swap    sw              0       0
{% endhighlight %}

Sauvez et quittez. Entrez à présent `sudo geany /etc/initramfs-tools/resume` pour la même opération. Votre fichier fini ressemble à :

{% highlight bash %}
RESUME=UUID=47ebbb16-aa14-41df-a003-b483141bb5f7
{% endhighlight %}

Encore une saisie de `sudo mkswap -U 47ebbb16-aa14-41df-a003-b483141bb5f7 /dev/sda6` dans mon cas (remplacez évidemment les UUID et les lecteurs correspondants à votre installation). L'option `-U` créé une swap en forçant l'UUID, et nécessite malgré tout le nom du lecteur.

Il ne reste qu'à compiler notre nouvelle configuration avec `sudo update-initramfs -u -k $(uname -r)` et redémarrer immédiatement. 

La même démarche est à suivre sur l'ensemble de vos distributions présentes sur votre disque.
