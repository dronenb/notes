# `ffmpeg`

## Convert Audible `.aax` to `.m4b`

- First you must obtain your 4 byte key. This is a per-account key, not per-file, so you only must obtain it once.
  - Any `.aax` file can be uploaded [here](https://audible-tools.kamsker.at/) to obtain this key.
- Then, you can use the following command to convert the file (this includes metadata and artwork):

```bash
filename="myfilename.aax"
new_filename=$(echo "${filename}" | sed -E 's/\.aax$/\.m4b/')
ffmpeg -y -activation_bytes <code> -i  "${filename}" -codec copy "${new_filename}"
```

I use the following script to convert all `.aax` files in `pwd` to `.m4b`:

```bash
ORIG_IFS="${IFS}"
IFS=$'\n'
for filename in $(find . -regex ".*\.aax" -type f); do
  new_filename=$(echo "${filename}" | sed -E 's/\.aax$/\.m4b/')
  activation_bytes=$(bw get item "Audible Key" | jq -r '.login.password')
  ffmpeg -y -activation_bytes "${activation_bytes}" -i  "${filename}" -codec copy "${new_filename}"
done
IFS="${ORIG_IFS}"
```
