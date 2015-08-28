---
title: О пользе дебага и документации
layout: post
tags: [programming, perl, getopt]
---

Есть простенький код на Перле:  

    use strict;
    use warnings;

    use Getopt::Long;

    sub parse {
        my $option_one = '';
        GetOptions("option_one=s" => \$option_one);
        print "option_one = $option_one\n";
        return;
    }

    my $option = '';
    parse();
    GetOptions("option=s" => \$option);
    print "option = $option\n";

    exit 0;


Проблема:  

    $ perl getopt.pl --option fd
    option_one = fd
    option =


А хотелось бы видеть значение у `$option`. Что за фигня?  
Включаем конфиг `Getopt`'а, включаем `debug`, взгляд сразу цепляется за `autoabbrev=1`, выключаю: `use Getopt::Long qw(:config no_autoabbrev);`, получаю:  

    $ perl getopt.pl --option fd
    Unknown option: option
    option_one =
    option =


ОК, Перл, давай `use Getopt::Long qw(:config no_autoabbrev pass_through);`:  

    $ perl getopt.pl --option fd
    option_one =
    option = fd

    $ perl getopt.pl --option fd --option_one 3
    option_one = 3
    option = fd

Именно то, чего жаждала душа.


PS Выдержка из документации:  

    auto_abbrev
    Allow option names to be abbreviated to uniqueness.
    Default is enabled unless environment variable POSIXLY_CORRECT
    has been set, in which case auto_abbrev is disabled.

