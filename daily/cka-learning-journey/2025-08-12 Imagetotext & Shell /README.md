
## 🔧 Activity- A simple shell script that copies the contents of screenshot image as text to clipboard  


1. Created a script to convert macOS screenshots to text using OCR.

2. Explored setting up a shell alias/script (snp2txt) for converting screenshots to text using macOS and clipboard workflows.

3. Investigated storing scripts in a shared path to work in both bash and zsh.

4. Tested macOS screenshot saving defaults, including saving to Desktop and clipboard simultaneously.

5. Learned about macOS temporary file security restrictions (.tmp, /private/var/folders/...) other safe temporary storage options (/tmp, /var/tmp, or Python's tempfile module).

6. Set up script execution so it works in both zsh and bash shells via #!/usr/bin/env sh.

## 🔍 Key Learnings

- Shell portability: Using #!/usr/bin/env sh allows the same script to work in zsh and bash.

- Clipboard contents: macOS clipboard can be inspected with pbpaste (text) or via AppleScript/Preview for images.

- Temporary file handling: macOS uses /private/tmp for temp storage; Python’s tempfile module can create safe temp files.

- The clipboard can hold high-quality image data, but Apple’s security model prevents reading certain .tmp files directly from private directories.

- /var/tmp retains files across reboots, /tmp clears on reboot — useful for choosing temp storage based on need.

## <0001f9ea> Commands / Configs

### bashrc or bash_profile or .zshrc

``alias snap2txt="$HOME/projects/imagetotext/snap2txt.sh"``

### Image contents to text processing code 
```
#!/usr/bin/env sh

# snp2txt - paste screenshot from clipboard -> OCR -> copy text -> optional cleanup


TIMESTAMP="$(date +'%Y%m%d_%H%M%S')"
IMG_PATH="$RESULT_DIR/screenshot_${TIMESTAMP}.png"
TXT_PATH="$RESULT_DIR/screenshot_${TIMESTAMP}.txt"

# 1) Save image from clipboard
echo "📸 Saving screenshot to: $IMG_PATH"
pngpaste "$IMG_PATH" || { echo "❌ No image found in clipboard."; exit 1; }

# 2) OCR (preserve spacing)
echo "🔍 Running OCR (psm 6, preserve_interword_spaces)..."
tesseract "$IMG_PATH" "${TXT_PATH%.txt}" --psm 6 -c preserve_interword_spaces=1 >/dev/null 2>&1 \
  || { echo "❌ OCR failed."; exit 1; }

# 3) Copy OCR text to clipboard (preserve formatting)
pbcopy < "$TXT_PATH"
echo "✅ OCR done. Text copied to clipboard."
echo "   Image: $IMG_PATH"
echo "   Text : $TXT_PATH"

# 4) Keep or Delete Files
IMG_SIZE=$(get_size "$IMG_PATH")
printf "Keep the image file (%s, %s)? [Y/n]: " "$IMG_PATH" "$IMG_SIZE"
read -r KEEP_IMG
KEEP_IMG="${KEEP_IMG:-Y}"
if [[ "$KEEP_IMG" =~ ^[Nn]$ ]]; then
  rm -f "$IMG_PATH" && echo "🗑️ Image deleted."
else
  echo "🟩 Image kept."
fi

RESULT_DIR="$HOME/projects/imagetotext/results"
mkdir -p "$RESULT_DIR"

# Get human-readable size of a file
get_size() {
  if command -v stat >/dev/null 2>&1; then
    # For macOS (BSD stat)
    stat -f "%z" "$1" 2>/dev/null | awk '{ split("B KB MB GB TB", units); s=$1; for(i=1;s>=1024;i++) s/=1024; printf "%.2f %s", s, units[i] }'
  else
    # For Linux (GNU stat)
    stat --printf="%s" "$1" 2>/dev/null | awk '{ split("B KB MB GB TB", units); s=$1; for(i=1;s>=1024;i++) s/=1024; printf "%.2f %s", s, units[i] }'
  fi
}

TXT_SIZE=$(get_size "$TXT_PATH")
printf "Keep the text file (%s, %s)? [Y/n]: " "$TXT_PATH" "$TXT_SIZE"
read -r KEEP_TXT
KEEP_TXT="${KEEP_TXT:-Y}"
if [[ "$KEEP_TXT" =~ ^[Nn]$ ]]; then
  rm -f "$TXT_PATH" && echo "🗑️ Text deleted."
else
  echo "🟩 Text kept."
fi

exit 0

```
