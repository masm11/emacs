# Using the gentoo ebuild

Add the repository:
```
gentoo_repo_path="$(realpath .)" # Path to the cloned repo + "/gentoo"

cat > /etc/portage/repos.conf/masm11-emacs.conf <<EOF
[masm11-emacs]
location = ${gentoo_repo_path}
auto-sync = no
EOF
```

Unmask it and set useflags for supporting only Wayland:
```
echo '=app-editors/emacs-9999 **' >> /etc/portage/package.accept_keywords/zz_main
echo '=app-editors/emacs-9999 -X cairo dynamic-loading' >> /etc/portage/package.use/main
echo '=app-editors/emacs-9999 -cairo' >> /etc/portage/profile/package.use.mask
```

Install it:
```
emerge emacs::masm11-emacs
```
