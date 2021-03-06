# Swag

[![Build Status](https://travis-ci.org/narkisr/swag.png)](https://travis-ci.org/narkisr/swag)

A DSL for documenting [Compojure](https://github.com/weavejester/compojure) routes using [Swagger](https://developers.helloreverb.com/swagger/) spec, such a spec can be viewed (and invoked) via [Swagger UI](https://github.com/wordnik/swagger-ui).


## Usage

```clojure
[swag "0.2.2"]
```

Example (see example folder for complete listing):

```clojure

; custom data types https://github.com/wordnik/swagger-core/wiki/Datatypes

(defmodel action :operates-on :string :src :string :actions {:type "Actions"})

(defroutes- actions {:path "/actions" :description "Adhoc actions managment"}

  ; here we use the custom action model (the model schema will reflect that).
  (POST- "/action" [& ^:action action] {:nickname "addActions" :summary "Adds an actions set"}
         {:status 200 :body (str "got action " action)})

  ;; ...

  ; note the use of :errorResponses
  (DELETE- "/action/:id" [^:int id] {:nickname "deleteActions" :summary "Deletes an action set" 
                                     :errorResponses (errors {:bad-req "Missing action"})}
        {:status 200 :body (str "got id " id)}))

```

For docs see:

 * API [docs](http://narkisr.github.io/swag/index.html)
 * Swagger spec [wiki](https://github.com/wordnik/swagger-core/wiki)

## Limitations

 * The DSL wraps Compojure and supports vector based destructing:

```clojure
 
  ; will not work because swag expects a vector of args with metadata types
  [:as {{id :id} :params}]

  ; however the folloing is compatible 
  [^:int id]
  
  ; will also work
  [& :sometype foo]
 
```

 * Parameter [attributes](https://github.com/wordnik/swagger-core/wiki/parameters) aren't supported yet this will be resolved in next release.


## License

Copyright © 2013 Ronen Narkis

Distributed under the Eclipse Public License, the same as Clojure.
