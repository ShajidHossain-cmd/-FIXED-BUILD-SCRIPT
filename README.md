# -FIXED-BUILD-SCRIPT
Just copy and paste the fixed version to your projets build_release.sh files



***********FOR THE WP2Satic MAIN PLUGIN build.release.sh ******************************
#!/usr/bin/env bash

######################################
##
## Build WP2Static for wp.org
##
## script archive_name dont_minify
##
## places archive in $HOME/Downloads
##
######################################

# check for dependencies
if ! command -v 7z > /dev/null
then
    echo "7z not available on your system, please install and retry" >&2
    exit 1
fi

# run script from project root
EXEC_DIR="$(pwd)"

TMP_DIR="$HOME/plugintmp"
rm -Rf "$TMP_DIR"
mkdir -p "$TMP_DIR"

rm -Rf "$TMP_DIR/wp2static"
mkdir "$TMP_DIR/wp2static"

# clear dev dependencies
rm -rf "$EXEC_DIR/vendor/*"
# load prod deps and optimize loader
composer install --quiet --no-dev --optimize-autoloader

# cp all required sources to build dir
cp -r "$EXEC_DIR"/src "$TMP_DIR"/wp2static/
cp -r "$EXEC_DIR"/vendor "$TMP_DIR"/wp2static/
cp -r "$EXEC_DIR"/views "$TMP_DIR"/wp2static/
cp -r "$EXEC_DIR"/css "$TMP_DIR"/wp2static/
cp -r "$EXEC_DIR"/js "$TMP_DIR"/wp2static/
cp -r "$EXEC_DIR"/*.php "$TMP_DIR"/wp2static/

cd "$TMP_DIR" || exit

# tidy permissions
find . -type d -exec chmod 755 {} +
find . -type f -exec chmod 644 {} +

# Create the archive using 7z
7z a -tzip -mx=9 "$1.zip" ./wp2static

cd - || exit

mkdir -p "$HOME/Downloads/"

cp "$TMP_DIR/$1.zip" "$HOME/Downloads/"

# reset dev dependencies
cd "$EXEC_DIR" || exit
# clear dev dependencies
rm -rf "$EXEC_DIR/vendor/*"
# load prod deps
composer install --quiet



-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



***********FOR THE Netlify Deployment Add-On build.release.sh ******************************



#!/bin/bash

############################################
##
## Build WP2Static Netlify Deployment Addon
##
## script archive_name dont_minify
##
## places archive in $HOME/Downloads
##
############################################

# run script from project root
EXEC_DIR="$(pwd)"

TMP_DIR="$HOME/plugintmp"
rm -Rf "$TMP_DIR"
mkdir -p "$TMP_DIR"

rm -Rf "$TMP_DIR/wp2static-addon-netlify"
mkdir "$TMP_DIR/wp2static-addon-netlify"

# clear dev dependencies
rm -Rf "$EXEC_DIR/vendor/*"
# load prod deps and optimize loader
composer install --quiet --no-dev --optimize-autoloader

# cp all required sources to build dir
cp -r "$EXEC_DIR/wp2static-addon-netlify.php" "$TMP_DIR/wp2static-addon-netlify/"
cp -r "$EXEC_DIR/src" "$TMP_DIR/wp2static-addon-netlify/"
cp -r "$EXEC_DIR/vendor" "$TMP_DIR/wp2static-addon-netlify/"
cp -r "$EXEC_DIR/views" "$TMP_DIR/wp2static-addon-netlify/"

cd "$TMP_DIR" || exit

# tidy permissions
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;

# Create the archive using 7z
7z a -tzip -mx=9 "$1.zip" "./wp2static-addon-netlify/*"

cd - || exit

mkdir -p "$HOME/Downloads/"

cp "$TMP_DIR/$1.zip" "$HOME/Downloads/"

# reset dev dependencies
cd "$EXEC_DIR" || exit
# clear dev dependencies
rm -Rf "$EXEC_DIR/vendor/*"
# load prod deps
composer install --quiet






















