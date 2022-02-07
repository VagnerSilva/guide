TMPDIR ?= /var/tmp

all:
	@echo Run \'sudo make install\' to install.

install:
	@echo Online install for Ubuntu WSL2 systemd Hack
	kali-wsl2-systemd-script.sh | bash
	@echo Run \'sudo make disable\' to disable systemd hack
	@echo Run \'sudo make uninstall\' to uninstall systemd hack

uninstall:
  @echo Online uninstall for Ubuntu WSL2 systemd Hack
  uninstall.sh | bash

disable:
  @echo Offline systemd Hack disable
  @sudo grep -v "source /usr/sbin/start-systemd-namespace" /etc/zsh/zshenv > $(TMPDIR)/tmpfile && mv $(TMPDIR)/tmpfile /etc/zsh/zshenv
  @echo Run \'sudo make uninstall\' to fully uninstall