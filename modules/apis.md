## APIs

---

## Consideraciones para desarrollar APIs

* Respetar estandarizaciones como [OpenAPI](https://www.openapis.org/)
  * Los estándares se apegan a implementar una API RESTful Hypermedia (HATEOAS:
Hypermedia as the Engine of Application State)
  * Ayudarse de herramientas como [Swagger](https://swagger.io/)

---

## Seguridad de las APIs

* Autorización de servicios: [OAuth2](https://oauth.net/2/)
  * No es autenticación, sólo autorización
  * Flujos de OAuth2: Implicit Flow, Authorization Code (+PKCE)
  * Barear tokens: _The name “Bearer authentication” can be understood as “give
    access to the bearer of this token.”_
* Autenticación basada en [OIDC](https://openid.net/connect/).
  * Capa de identidad por sobre OAuth2
* API gateways: conceptos

---

## Microservicios

* No confundir API monolítica con microservicios
* Con microservicios aparecen nuevos problemas:
  * Tracing
  * Transacciones distribuidas
* Leer más de [microservicios acá](https://microservices.io)
