---
title: EL Implicit Objects
category: Java
excerpt: |
  Java Expression Language (EL) adalah mekanisme yang memungkinkan komunikasi secara dinamis aplikasi berbasis JSP/JSF, kita menggunakan ekspresi ini di presentation layer untuk dihubungkan dengan application logic layer.
feature_text: |
  ## Java Expressions Language EL
  Akses Dinamis ke Data Aplikasi JSF melalui Bahasa Ekspresi (Expression Language)
feature_image: "https://unsplash.it/1200/400?image=1048"
image: "https://unsplash.it/1200/400?image=1048"

---

JSF menyediakan beberapa objek yang berhubungan dengan *current request* dan *environment*. EL mengekspose beberapa objek ini (dikenal dengan *implicit object*) yang bisa diakses saat *runtime* di *Facelet*, *servlet*, atau *backing bean*, objek ini bisa diakses melalui *value expression* dan di manage oleh container. Untuk masing-masing expression, EL pertama kali me-ngecek jika value *base*nya adalah salah satu dari implicit objek dan jika tidak kemudian akan memeriksa bean secara bertahap ke scope yang lebih luas (dari *request scope* ke *view scope* hingga ke *application scope*).

Di dalam EL, bagian dari expression sebelum dot atau square bracket disebut **base** dan itu biasanya mengindikasikan dimana lokasi bean nya. Bagian setelah dot pertama, atau square bracket disebut **property** dan secara rekursif dipecah ke bagian yang lebih kecil yang me-representasikan property bean yang di dapat dari base.

Penjelasan singkat beberapa objek implisit:

Impicit object EL | Type | Deskripsi
---|---|---
#{application} | ServletContext atau PortletContext | instance dari ServletContext atau PortletContext
#{facesContext} | FacesContext | instance FacesContext
#{initParam} | Map | context map parameter inisialisasi yang di kembalikan oleh getInitParameterMap
#{session} | HttpSession / PortletSession | instance HttpSession atau PortletSession
#{view} | UIViewRoot | the current UIViewRoot (the root of the UIComponent tree)
#{component} | UIComponent | This is the current UIComponent
#{cc} | UIComponent | composite component yang sedang di proses
#{request} | ServletRequest / PortletRequest | This	is an instance of ServletRequest or PortletRequest
#{applicationScope} | Map | map	to store application-scoped data returned by getApplicationMap
#{sessionScope} | Map | map	to store session-scoped data returned by getSessionMap
#{viewScope} | Map | map to store current view scoped data returned by getViewMap
#{requestScope} | Map | map	to store request-scoped data returned by getRequestMap
#{flowScope} | Map | map to store flow-scoped data returned by facesContext.getApplication().getFlowHandler().getCurrentFlowScope()
#{flash} | Map | This is a map that contains values present only on the “next” request.
#{param} | Map | This is a map view of all the query parameters for this request. It is returned by getRequestParameterMap
#{paramValues} | Map | This is the request parameter value map returned by getRequestParameterValuesMap
#{header} | Map | This is a map view of all the HTTP headers for this request returned by getRequestHeaderMap
#{headerValue} | Map | This is the request header values map returned by getRequestHeaderValuesMap Each value in the map is an array of strings that contains all the values for that key
#{cookie} | Map | This is a map view of values in the HTTP Set-Cookie header returned by getRequestCookieMap
#{resource} | Resource | This is a JSF resource identifier to a concrete resource URL

