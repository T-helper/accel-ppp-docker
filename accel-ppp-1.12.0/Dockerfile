FROM debian:bullseye-slim AS build

RUN apt-get update && apt-get install -y git build-essential cmake gcc git libpcre3-dev libssl-dev libsnmp-dev linux-libc-dev

RUN git clone --branch 1.12.0 --depth 1  https://github.com/accel-ppp/accel-ppp.git /usr/src/accel-ppp

WORKDIR /usr/src/accel-ppp/build
RUN cmake \
        -DRADIUS=FALSE \
        -DNETSNMP=FALSE \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DBUILD_DRIVER=TRUE \
        ..

RUN make install


FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y ppp pppoe

COPY --from=build /usr/sbin/accel-pppd /usr/sbin/accel-pppd
COPY --from=build /usr/bin/accel-cmd /usr/bin/accel-cmd
COPY --from=build /usr/share/accel-ppp /usr/share/accel-ppp
COPY --from=build /usr/lib64/accel-ppp /usr/lib64/accel-ppp

CMD ["/usr/sbin/accel-pppd", "-c", "/etc/accel-ppp.conf"]
