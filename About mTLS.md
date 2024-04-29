mTLS(mutual TLS)

-   Upgraded version of TLS(SSL)

-   Automatically applied when developer uses service mesh

-   Has more handshake than TLS(server => client)
-   mTLS(server <=> client)

-   Visiting all sidecars(proxy) cause low performance.
-   When microservice A(with mTLS) needs to communicate with microservice B(non mTLS) should communicate with normal text(non secure)
