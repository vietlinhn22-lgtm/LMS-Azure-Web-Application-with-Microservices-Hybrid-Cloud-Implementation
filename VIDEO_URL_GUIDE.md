# Video URL Guide for LMS

## 📺 Supported Video Formats

### ✅ **WORKING: Google Drive Videos**

**Format:** `https://drive.google.com/file/d/{FILE_ID}/preview`

**How to get the correct URL:**
1. Upload video to Google Drive
2. Right-click → Get link
3. Set sharing to "Anyone with the link"
4. Copy the link (format: `https://drive.google.com/file/d/FILE_ID/view`)
5. Use it as-is in the LMS (system will auto-convert to preview format)

**Example:**
```
Original: https://drive.google.com/file/d/18hCoZsjWQ5QIKdnKCd7lKklrijHoSAt_/view
Used in LMS: https://drive.google.com/file/d/18hCoZsjWQ5QIKdnKCd7lKklrijHoSAt_/view
System converts to: https://drive.google.com/file/d/18hCoZsjWQ5QIKdnKCd7lKklrijHoSAt_/preview
```

**✅ Status:** Embeds and plays directly in LMS

---

### ⚠️ **LIMITED: YouTube Videos**

**Formats:**
- `https://www.youtube.com/watch?v={VIDEO_ID}`
- `https://youtu.be/{VIDEO_ID}`

**Issue:** Most YouTube videos CANNOT be embedded due to:
- Creator disabled embedding
- Copyright restrictions
- YouTube policies

**Current Solution:**
- Shows attractive "Click to Open YouTube" button
- Opens video in new tab on YouTube.com
- Best user experience for restricted videos

**Example:**
```
URL: https://youtu.be/eqg0STvbp1g
Result: Shows button → Opens in YouTube
```

**❌ Status:** Cannot force embedding if creator disabled it

---

### ✅ **ALTERNATIVE: Direct MP4 URLs**

**Format:** Direct link to `.mp4` file

**Examples:**
```
https://example.com/videos/lecture.mp4
https://yourserver.com/content/tutorial.mp4
```

**✅ Status:** Embeds perfectly with HTML5 video player

---

### ✅ **WORKING: Vimeo Videos**

**Format:** `https://vimeo.com/{VIDEO_ID}`

**Example:**
```
https://vimeo.com/123456789
System converts to: https://player.vimeo.com/video/123456789
```

**✅ Status:** Usually embeds well (Vimeo is embedding-friendly)

---

## 🎯 **Recommendations**

### For Best Video Experience:

1. **BEST OPTION: Upload to Google Drive**
   - ✅ Reliable embedding
   - ✅ Free storage
   - ✅ Good playback quality
   - ✅ Easy sharing

2. **ALTERNATIVE: Direct MP4 Upload**
   - ✅ Best control
   - ✅ No third-party dependencies
   - ❌ Requires your own hosting

3. **USE WITH CAUTION: YouTube**
   - ⚠️ May not embed
   - ✅ Students can still watch (opens YouTube)
   - ✅ Good for optional resources

---

## 🔧 **Troubleshooting**

### Google Drive "Refused to Connect"
**Solution:**
1. Check sharing settings: Must be "Anyone with the link"
2. Ensure file is not too large (>100MB may have issues)
3. Try using `/preview` format instead of `/view`
4. Wait a few minutes after uploading

### YouTube "Video Unavailable"
**Solution:**
- This is normal if embedding is disabled
- Students can click button to watch on YouTube
- Consider using Vimeo or Google Drive instead

### Video Won't Play
**Solution:**
1. Test the URL in a regular browser first
2. Check if URL is publicly accessible
3. Verify file format is MP4, WebM, or OGG
4. For Google Drive, ensure sharing is enabled

---

## 📝 **Quick Setup Checklist**

- [ ] Video uploaded to Google Drive
- [ ] Sharing set to "Anyone with the link"
- [ ] URL copied (with /view or /preview)
- [ ] URL pasted in LMS lesson
- [ ] Tested in student view

---

## 🚀 **Current LMS Features**

✅ **Google Drive**: Full embedding with preview player  
✅ **Direct MP4**: HTML5 video player  
✅ **Vimeo**: Embedded Vimeo player  
⚠️ **YouTube**: Button to open on YouTube (most videos cannot embed)  
✅ **Error Handling**: Clear messages when videos can't load  
✅ **Mobile Friendly**: Works on phones and tablets
