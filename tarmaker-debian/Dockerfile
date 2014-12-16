FROM debian:wheezy
RUN apt-get update -q
RUN apt-get install -qy bzip2 make gcc pkg-config libselinux1-dev libpam0g-dev
ENV BUSYBOX_VERSION 1.22.1
RUN wget -O - http://busybox.net/downloads/busybox-$BUSYBOX_VERSION.tar.bz2 | tar jxf -
RUN cd busybox-$BUSYBOX_VERSION && make defconfig && LDFLAGS="--static" make && make install 
RUN mkdir /rootfs
WORKDIR /rootfs
RUN mkdir bin etc dev dev/pts lib proc sys tmp root
RUN touch etc/resolv.conf
RUN cp /etc/nsswitch.conf etc/nsswitch.conf
RUN echo root:x:0:0:root:/root:/bin/sh > etc/passwd
RUN echo root:x:0: > etc/group
RUN echo daemon:x:1:1:daemon:/usr/sbin:/bin/sh >> etc/passwd
RUN echo daemon:x:1: >> etc/group
RUN echo wheel:x:10:root >> etc/group
RUN ln -s lib lib64
RUN ln -s bin sbin
ENV BUSYBOX /busybox-$BUSYBOX_VERSION/_install/bin/busybox
RUN cp $BUSYBOX bin
RUN cp $BUSYBOX /bin && /bin/busybox --install -s bin
RUN bash -c "cp /lib/s390x-linux-gnu/lib{c,m,dl,rt,nsl,nss_*,pthread,resolv}.so.* lib"
RUN cp /lib/s390x-linux-gnu/ld64.so.1 lib
RUN tar cf /rootfs.tar .
RUN for X in console null ptmx random stdin stdout stderr tty urandom zero ; do tar uf /rootfs.tar -C/ ./dev/$X ; done
