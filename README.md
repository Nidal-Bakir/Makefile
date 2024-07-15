# makefile

### Useful Makefiles for: 
* Backend
* Frontend
* Mobile App Dev
* Any

## Go-lang
``` Makefile
include .env
.DEFAULT_GOAL := build

build:
	@echo "Building..."
	@go build -o main cmd/api/main.go

# Run the application
run:
	@go run cmd/api/main.go

# Create DB container
docker-run:
	@docker compose up
 
# Shutdown DB container
docker-down:
	@docker compose down

# Test the application
test:
	@echo "Testing..."
	@go test ./tests -v

# Live Reload
watch:
	air; \
	echo "Watching...";\

# goose db migrations binary utils
GOOSE_CMD=GOOSE_MIGRATION_DIR=${GOOSE_MIGRATION_DIR} GOOSE_DRIVER=${GOOSE_DRIVER} GOOSE_DBSTRING=${GOOSE_DBSTRING} goose

# Migrate the DB to the most recent version available
goose-up:
	@${GOOSE_CMD} up

# Roll back the version by 1
goose-up-by-one :
	@${GOOSE_CMD} up-by-one

# Migrate the DB to a specific VERSION
goose-up-to :
	@read -p "version: " version; ${GOOSE_CMD} up-to $$version

# Roll back the version by 1
goose-down:
	@${GOOSE_CMD} down

# Roll back to a specific VERSION
goose-down-to :
	@read -p "version: " version; ${GOOSE_CMD} down-to $$version

# Re-run the latest migration
goose-redo :
	@${GOOSE_CMD} redo

# Roll back all migrations
goose-reset :
	@${GOOSE_CMD} reset

# Dump the migration status for the current DB
goose-status:
	@${GOOSE_CMD} status

# Print the current version of the database
goose-version:
	@${GOOSE_CMD} version

# Apply sequential ordering to migrations
goose-fix:
	@${GOOSE_CMD} fix

# Clean the binary
clean:
	@echo "Cleaning..."
	@rm -f main
	@echo "Done"

```
## Flutter
``` Makefile
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
```




