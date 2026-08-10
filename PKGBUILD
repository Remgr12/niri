pkgname=niri-custom
pkgver=26.4.0
pkgrel=1
pkgdesc="A scrollable-tiling Wayland compositor (Custom fork)"
arch=('x86_64')
url="https://github.com/YaLTeR/niri"
license=('GPL3')
depends=('cairo' 'glib2' 'libinput' 'libxkbcommon' 'mesa' 'pango' 'pixman' 'seatd' 'systemd-libs' 'wayland')
makedepends=('cargo' 'git' 'wayland-protocols' 'clang')
provides=('niri')
conflicts=('niri')

build() {
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR="$srcdir/target"
  cd "$srcdir/.."
  chmod 755 "$srcdir/../pkg" || true
  cargo build --release --locked
}

package() {
  cd "$srcdir/.."
  install -Dm755 "$srcdir/target/release/niri" "$pkgdir/usr/bin/niri"
  
  if [ -f "$srcdir/target/release/niri-msg" ]; then
    install -Dm755 "$srcdir/target/release/niri-msg" "$pkgdir/usr/bin/niri-msg"
  fi

  # Desktop file wrapped with prime-run
  mkdir -p "$pkgdir/usr/share/wayland-sessions"
  cat <<EOF > "$pkgdir/usr/share/wayland-sessions/niri.desktop"
[Desktop Entry]
Name=Niri
Comment=A scrollable-tiling Wayland compositor
Exec=prime-run /usr/bin/niri --session
Type=Application
DesktopNames=niri
EOF

  if [ -f resources/niri-portals.conf ]; then
    install -Dm644 resources/niri-portals.conf -t "$pkgdir/usr/share/xdg-desktop-portal/"
  fi
  
  if [ -f resources/niri.service ]; then
    install -Dm644 resources/niri.service -t "$pkgdir/usr/lib/systemd/user/"
  fi
  if [ -f resources/niri-shutdown.target ]; then
    install -Dm644 resources/niri-shutdown.target -t "$pkgdir/usr/lib/systemd/user/"
  fi
}
