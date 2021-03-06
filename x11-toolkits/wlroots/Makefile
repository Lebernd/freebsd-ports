# $FreeBSD$

PORTNAME=	wlroots
DISTVERSION=	0.11.0
CATEGORIES=	x11-toolkits

PATCH_SITES=	https://github.com/${GH_ACCOUNT}/${GH_PROJECT}/commit/
PATCHFILES+=	0fe36eb89754.patch:-p1 # https://github.com/swaywm/wlroots/pull/2064
PATCHFILES+=	90cb6d26877b.patch:-p1 # https://github.com/swaywm/wlroots/pull/2064

MAINTAINER=	jbeich@FreeBSD.org
COMMENT=	Modular Wayland compositor library

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	evdev-proto>0:devel/evdev-proto \
		wayland>=1.18:graphics/wayland \
		wayland-protocols>=1.17:graphics/wayland-protocols
LIB_DEPENDS=	libudev.so:devel/libudev-devd \
		libdrm.so:graphics/libdrm \
		libwayland-egl.so:graphics/wayland \
		libinput.so:x11/libinput \
		libxkbcommon.so:x11/libxkbcommon
RUN_DEPENDS=	libudev-devd>=0.4.1:devel/libudev-devd \
		mesa-dri>0:graphics/mesa-dri

USES=		compiler:c11 gl meson pkgconfig xorg
USE_GITHUB=	yes
USE_GL=		egl gbm glesv2
USE_XORG=	pixman
USE_LDCONFIG=	yes
GH_ACCOUNT=	swaywm
MESON_ARGS=	-Dexamples=false

# XXX Drop after FreeBSD 11.3 EOL (aarch64)
.if exists(/usr/lib/clang/8.0.0)
# https://github.com/swaywm/wlroots/issues/1450
LLD_UNSAFE=	yes
.endif

OPTIONS_DEFINE=	CONSOLEKIT LIBSEAT X11
OPTIONS_DEFAULT=X11
OPTIONS_SUB=	yes

CONSOLEKIT_DESC=	DRM sessions without root using ConsoleKit2
CONSOLEKIT_PATCH_SITES=	https://github.com/myfreeweb/${GH_PROJECT}/commit/:ck2
CONSOLEKIT_PATCHFILES=	b59460944208.patch:-p1:ck2 # https://github.com/swaywm/wlroots/pull/1467
CONSOLEKIT_LIB_DEPENDS=	libdbus-1.so:devel/dbus \
			libconsolekit.so:sysutils/consolekit2
CONSOLEKIT_MESON_ENABLED=	consolekit2
.if make(makesum)
.MAKEFLAGS:		WITH+=CONSOLEKIT
.endif

LIBSEAT_DESC=		DRM sessions with/without root via libseat (experimental)
LIBSEAT_PATCH_SITES=	https://github.com/${GH_ACCOUNT}/${GH_PROJECT}/commit/:libseat
LIBSEAT_PATCHFILES=	be3753a4774f.patch:-p1:libseat # https://github.com/swaywm/wlroots/pull/2364
LIBSEAT_PATCHFILES+=	c1955ad1ab05.patch:-p1:libseat # FreeBSD build fix
LIBSEAT_LIB_DEPENDS=	libseat.so:sysutils/seatd
#LIBSEAT_MESON_ENABLED=	libseat
.if make(makesum)
.MAKEFLAGS:		WITH+=LIBSEAT
.endif

X11_LIB_DEPENDS=libxcb-errors.so:x11/xcb-util-errors \
		libxcb-icccm.so:x11/xcb-util-wm
X11_RUN_DEPENDS=xwayland-devel>0:x11-servers/xwayland-devel
X11_USE=	XORG=x11,xcb
X11_MESON_ENABLED=x11-backend xcb-icccm xwayland

.include <bsd.port.mk>
