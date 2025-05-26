<!-- # My own locale

English (Europe) `en_150`

Should go in `/usr/share/i18n/locales/.`  
`-rw-r--r-- 1 root root` -->

# Linux English (Europe) Locale

Based on `i18n`, `en_DK`, `de_LI`, `en_US`.

I made this because there is no one locale that completely satisfies me.

I prefer metric, Euro, ISO 8601, period (`.`) as the decimal symbol and
apostrophe (`'`) for the thousands-separator.

## Installation (+ KDE Plasma Instructions)

By the way, I'm using Debian.

```bash
sudo cp en_150 /usr/share/i18n/locales/
```

Then edit `~/.config/plasma-localerc`; match its contents with the one
[here](./plasma-localerc).

KDE (or Qt or perl5) is very weird in that it kinda ignores the system's
locales. It has its own locales, some that don't exist (like `en_150`, `en_AT`,
`en_SE`, etc.) outside of KDE (something seems to be in
`/usr/share/perl5/DateTime/Locale/` though?). I live in Austria so for some stuff
I set `en_AT`. `en_SE` uses ISO 8601 for date and time, so I wanna use that.

`en_AT` and `en_SE` exist for KDE but not for perl or whatever, so to suppress
those error/warning messages, make pseudo-locales:

```bash
sudo cp copy_en_150 /usr/share/i18n/locales/en_AT
```

```bash
sudo cp copy_en_150 /usr/share/i18n/locales/en_SE
```

Now generate the locales:

```bash
sudo locale-gen
```

For me I see:

```bash
talon@talon-acerswift3-debian:~/Documents/MyOwnLocale$ sudo locale-gen
Generating locales (this might take a while)...
  C.UTF-8... done
  de_AT.UTF-8... done
  de_LI.UTF-8... done
  en_DK.UTF-8... done
  en_US.UTF-8... done
  en_150.UTF-8... done
  en_SE.UTF-8... done
  en_AT.UTF-8... done
Generation complete.
```

Also look at `/etc/locale.conf`:

```conf
LANG="en_150.UTF-8"
LANGUAGE="en_150:en"
LC_CTYPE="en_150.UTF-8"
LC_NUMERIC="en_150.UTF-8"
LC_TIME="en_150.UTF-8"
LC_COLLATE="en_150.UTF-8"
LC_MONETARY="en_150.UTF-8"
LC_MESSAGES="en_150.UTF-8"
LC_PAPER="en_150.UTF-8"
LC_NAME="en_150.UTF-8"
LC_ADDRESS="en_150.UTF-8"
LC_TELEPHONE="en_150.UTF-8"
LC_MEASUREMENT="en_150.UTF-8"
LC_IDENTIFICATION="en_150.UTF-8"
```

Now reboot.

When I run `locale` I see the following:

```bash
talon@talon-acerswift3-debian:~$ locale
LANG=en_150.UTF-8
LANGUAGE=en_150:en
LC_CTYPE=en_150.UTF-8
LC_NUMERIC=de_LI.UTF-8
LC_TIME=en_SE.UTF-8
LC_COLLATE=en_150.UTF-8
LC_MONETARY=en_150.UTF-8
LC_MESSAGES=en_150.UTF-8
LC_PAPER=en_150.UTF-8
LC_NAME=en_AT.UTF-8
LC_ADDRESS=en_AT.UTF-8
LC_TELEPHONE=en_AT.UTF-8
LC_MEASUREMENT=en_150.UTF-8
LC_IDENTIFICATION=en_150.UTF-8
LC_ALL=
```

## Further Reading

<https://manpages.debian.org/bookworm/manpages/locale.5.en.html>

<https://www.unicode.org/L2/L1999/n634.pdf>

<https://www.localeplanet.com/index.html>

<https://community.kde.org/KDE_Core/KLocale>

<https://forum.manjaro.org/t/kde-do-not-works-with-custom-locale/148440>

<https://discuss.kde.org/t/custom-regional-formats/20077/4>
