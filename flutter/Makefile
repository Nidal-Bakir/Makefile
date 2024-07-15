.DEFAULT_GOAL := stg

FLUTTER_BIN=flutter
DART_BIN=dart
STAGING_MAIN_PATH=lib/config/env/staging_main.dart
PRODUCTION_MAIN_PATH=lib/config/env/production_main.dart
LINE_LEN=120

STAGING_FLAVOR_NAME=staging
PRODUCTION_FLAVOR_NAME=production

KILL_JAVA_PROC=echo \\nKilling java proc.... \\n ;  killall --signal=KILL java ; echo \\njava proc was killed \\n

.PHONY: pub_get
pub_get:  
	$(FLUTTER_BIN) pub get
	
.PHONY: fix
fix: pub_get
	$(DART_BIN) fix --apply

.PHONY: format
format: fix
	$(DART_BIN) format ./ --line-length=$(LINE_LEN)

.PHONY: analyze
analyze: format
	$(DART_BIN) analyze ./ 

.PHONY: stg
stg: analyze
	echo \\n "--->" Building $(STAGING_FLAVOR_NAME) APK \\n ; $(KILL_JAVA_PROC) ; $(FLUTTER_BIN) build apk --release --flavor $(STAGING_FLAVOR_NAME) -t $(STAGING_MAIN_PATH) ; $(KILL_JAVA_PROC) ; echo "DONE......"

.PHONY: prod
prod: analyze
	echo \\n "--->" Building $(PRODUCTION_FLAVOR_NAME) APK \\n ; $(KILL_JAVA_PROC) ; $(FLUTTER_BIN) build apk --release --flavor $(PRODUCTION_FLAVOR_NAME) -t $(PRODUCTION_MAIN_PATH) ; $(KILL_JAVA_PROC) ; echo "DONE......"

.PHONY: aab
aab: analyze
	echo \\n "--->" Building $(PRODUCTION_FLAVOR_NAME) AAB \\n ; $(KILL_JAVA_PROC) ; $(FLUTTER_BIN) build appbundle --flavor $(PRODUCTION_FLAVOR_NAME) -t $(PRODUCTION_MAIN_PATH) ; $(KILL_JAVA_PROC) ; echo "DONE......"
 

.PHONY: clean
clean:
	$(FLUTTER_BIN) clean && $(FLUTTER_BIN) pub get
