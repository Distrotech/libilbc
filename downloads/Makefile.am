# here are the file names and urls for the technical specification
technical_spec = rfc3951.txt
technical_spec_url = ftp://ftp.rfc-editor.org/in-notes/$(technical_spec)

# for the extract script
extract_script = extract-cfile
extract_script_url = http://www.ilbcfreeware.org/documentation/$(extract_script)

downloaded_files = $(technical_spec) $(extract_script) $(license)
checkmarks = $(downloaded_files:%=%.check)
success_marker = ../src/iLBC_define.h
BUILT_SOURCES = $(checkmarks) $(success_marker)

checksum_dir = $(srcdir)/known_checksums
checksum_files = $(addprefix $(checksum_dir)/, $(downloaded_files:%=%.md5))
EXTRA_DIST = $(checksum_files)

$(technical_spec):
	echo $(WGET) --no-check-certificate $(technical_spec_url) -O ${technical_spec}

$(extract_script):
	echo $(WGET) --no-check-certificate $(extract_script_url).txt -O ${extract_script}.awk


# for check, go to $(srcdir) or ./, depending on where file is
$(checkmarks): %.check: %
	cd $(<D); echo $(MD5SUM) -c $(checksum_dir)/$(<F).md5
	touch $@

# cleaning up in ../src is necessary because the awk script appends
$(success_marker): $(extract_script) $(technical_spec) $(checkmarks)
	cd ../src; rm *.c *.h &> /dev/null; $(AWK) -f $(CURDIR)/$< \
		$(CURDIR)/$(word 2, $+) > /dev/null

DISTCLEANFILES = $(BUILT_SOURCES)
