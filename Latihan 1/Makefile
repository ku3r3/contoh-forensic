.PHONY: unpack clean

unpack:
	@find . -name '*.zip' -print0 | bash -c 'while IFS= read -r -d "" zipfile; do \
		echo "Unzipping $$zipfile"; \
		( cd "$$(dirname "$$zipfile")" && unzip -o "$$(basename "$$zipfile")" ); \
	done'

clean:
	@find . -name '*.zip' -print0 | bash -c 'while IFS= read -r -d "" zipfile; do \
		echo "Cleaning files from $$zipfile"; \
		( cd "$$(dirname "$$zipfile")" && unzip -Z1 "$$(basename "$$zipfile")" | \
			sed "s|^\./||" | sort -r | \
			while IFS= read -r entry; do \
				if [ -e "$$entry" ] || [ -d "$$entry" ]; then \
					rm -rf -- "$$entry" 2>/dev/null || true; \
				fi; \
			done ); \
	done || true'

dist:
	make clean
	(cd ..; zip sleuthkit_test_data.zip sleuthkit_test_data/{Makefile,README.md,*/}
