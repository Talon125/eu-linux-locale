<!-- # My own locale

English (Europe) `en_150`

Should go in `/usr/share/i18n/locales/.`  
`-rw-r--r-- 1 root root` -->

# Linux English (Europe) Locale

Based on `i18n`, `en_DK`, `de_LI`, `en_US`.

I made this because there is no one locale that completely satisfies me.

I prefer metric, Euro, ISO 8601, period (`.`) as the decimal symbol and
apostrophe (`'`) for the thousands-separator.

I tried this on KDE on Debian Sid and Debian 12, as well as Linux Mint Cinnamon.

## Installation

- You're gonna want the files in this repository, at least `en_150` and
  `copy_en_150` for KDE. You can either download them manually, clone this
  repository, or download it as an archive and then extract it.

- Open up a terminal and **c**hange **d**irectory into where the files
  mentioned above are.

- Run the following command to add this locale to your other locales:

```bash
sudo cp en_150 /usr/share/i18n/locales/
```

---

### Extra steps for KDE Plasma

If you're not on KDE Plasma, then you may skip this section. Scroll down until
you see "END Extra steps for KDE Plasma".

KDE (or Qt or perl5) is very weird in that it kinda ignores the system's
locales. It has its own locales, some that don't exist (like `en_150`, `en_AT`,
`en_SE`, etc.) outside of KDE (they do seem to be in
`/usr/share/perl5/DateTime/Locale/` though? But only for DateTime?). I live in
Austria so for some stuff I set `en_AT`. `en_SE` uses ISO 8601 for date and
time, so I wanna use that.

If you want, you can use something other than `en_AT`, whatever Plasma offers.

- [Optional] Create a backup of your `plasma-localerc`:

```bash
cp ~/.config/plasma-localerc ~/.config/plasma-localerc.bak
```

- Now edit `~/.config/plasma-localerc` with your editor of choice to match the
  following:

```plasma-localerc
[Formats]
LANG=en_150.UTF-8
LC_ADDRESS=en_AT.UTF-8
LC_COLLATE=en_150.UTF-8
LC_CTYPE=en_150.UTF-8
LC_IDENTIFICATION=en_150.UTF-8
LC_MEASUREMENT=en_150.UTF-8
LC_MESSAGES=en_150.UTF-8
LC_MONETARY=en_150.UTF-8
LC_NAME=en_AT.UTF-8
LC_NUMERIC=de_LI.UTF-8
LC_PAPER=en_150.UTF-8
LC_TELEPHONE=en_AT.UTF-8
LC_TIME=en_SE.UTF-8

```

- `en_AT` and `en_SE` exist for KDE but not for perl or whatever, so to suppress
  those error/warning messages, make pseudo-locales:

```bash
sudo cp copy_en_150 /usr/share/i18n/locales/en_AT
```

```bash
sudo cp copy_en_150 /usr/share/i18n/locales/en_SE
```

(You can replace `en_AT` with what you want like I mentioned at the start of this
section)

### END Extra steps for KDE Plasma

---

- Enable the locale(s) so we can generate them by editing `/etc/locale.gen`. Be
  sure to add the following:

```locales.gen
en_150.UTF-8 UTF-8
```

- [KDE Plasma] If you're on KDE Plasma, also add the pseudo locales we made in
  that step, e.g.:

```locales.gen
en_AT.UTF-8 UTF-8
en_SE.UTF-8 UTF-8
```

- Now generate the locales by running this command:

```bash
sudo locale-gen
```

Your output may look something like this:

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

- Now to set the system locale variables. Edit the file `/etc/locale.conf` or
  `/etc/default/locale` depending on your distro/DE (some only have the latter)
  to contain the following:

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

- Now reboot your system.

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

<https://forums.opensuse.org/t/kde-region-language-shows-options-that-dont-work/163591>

<https://bugs.kde.org/show_bug.cgi?id=403580>

<https://github.com/sddm/sddm/issues/1570>
