# jsonista [![Continuous Integration status](https://secure.travis-ci.org/metosin/jsonista.png)](http://travis-ci.org/metosin/jsonista)

> *jsonissa / jsonista / jsoniin, jsonilla / jsonilta / jsonille*

Clojure library for fast JSON encoding and decoding.

* Explicit configuration
* Embrace Java for speed
* Uses [Jackson](https://github.com/FasterXML/jackson) directly
* [API docs](https://metosin.github.io/jsonista/)

Much faster than [Cheshire](https://github.com/dakrone/cheshire) while still having all the necessary features for web development. Designed for use with [Muuntaja](https://github.com/metosin/muuntaja).

Blogged:
* [Faster JSON processing with jsonista](http://www.metosin.fi/blog/faster-json-processing-with-jsonista/)

## Latest version

[![Clojars Project](http://clojars.org/metosin/jsonista/latest-version.svg)](http://clojars.org/metosin/jsonista)

Requires Java1.8+

## Quickstart

```clojure
(require '[jsonista.core :as j])

(j/write-value-as-string {"hello" 1})
;; => "{\"hello\":1}"

(j/read-value *1)
;; => {"hello" 1}
```

## More examples

Changing how map keys are encoded & decoded:

```clojure
(defn reverse-string [s] (apply str (reverse s)))

(def mapper
  (j/object-mapper
    {:encode-key-fn (comp reverse-string name)
     :decode-key-fn (comp keyword reverse-string)}))

(-> {:kikka "kukka"}
    (doto prn)
    (j/write-value-as-string mapper)
    (doto prn)
    (j/read-value mapper)
    (prn))
; {:kikka "kukka"}
; "{\"akkik\":\"kukka\"}"
; {:kikka "kukka"}
```

Reading & writing directly into a file:

```clojure
(def file (java.io.File. "hello.json"))

(j/write-value file {"hello" "world"})

(slurp file)
;; => "{\"hello\":\"world\"}"

(j/read-value file)
;; => {"hello" "world"}
```

## Performance

* All standard encoders and decoders are written in Java
* Protocol dispatch with `read-value` & `write-value`
* Jackson `ObjectMapper` is used directly
* Small functions to support JVM Inlining

See [perf-tests](/test/jsonista/json_perf_test.clj) for details.

![encode](/docs/encode.png)

![decode](/docs/decode.png)

## License

Copyright &copy; 2016-2018 [Metosin Oy](http://www.metosin.fi).

Distributed under the Eclipse Public License, the same as Clojure.
