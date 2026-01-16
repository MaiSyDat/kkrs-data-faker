# KKSR Data Faker

![Version](https://img.shields.io/badge/version-4.0.0-blue.svg)
![WordPress](https://img.shields.io/badge/WordPress-5.0%2B-green.svg)
![PHP](https://img.shields.io/badge/PHP-7.2%2B-purple.svg)
![License](https://img.shields.io/badge/license-GPL%20v2%2B-red.svg)

**Traffic-based ratings and sales counter for WordPress**. Automatically increments KK Star Ratings and WooCommerce sales based on unique visitor views with intelligent cooldown system.

## 🚀 Features

### Traffic-Based Counting
- **No Random Numbers**: Increments based on real unique visitors
- **Visitor Tracking**: Uses IP + User Agent hash for identification
- **Smart Cooldown**: Configurable period (default: 7 days) before same visitor counts again
- **F5 Protection**: Session-based prevention of refresh spamming
- **Singular Pages Only**: Only tracks on individual post/product pages, not archives

### Dual Functionality
- **Ratings Module**: Increments KK Star Ratings with random stars (1-5 configurable)
- **Sales Module**: Increments WooCommerce total sales count
- **Tabbed Admin**: Separate settings for each module
- **Independent Thresholds**: Set different limits for ratings and sales

### Smart Features
- **Automatic Average**: Calculates rating average based on random stars
- **Threshold Protection**: Stops incrementing at configured limits
- **Real Data Safe**: Never resets your existing ratings or sales
- **Clean Uninstall**: Removes tracking data but preserves real numbers

## 📋 Requirements

- WordPress 5.0+
- PHP 7.2+
- KK Star Ratings plugin
- WooCommerce (for sales module)

## 🔧 Installation

### Via WordPress Admin
1. Download plugin ZIP
2. Go to Plugins → Add New → Upload
3. Choose ZIP file and click Install
4. Activate plugin

### Manual Installation
```bash
cd wp-content/plugins/
git clone https://github.com/MaiSyDat/kksr-data-faker.git
# or upload extracted folder
```

Then activate via WordPress admin.

## ⚙️ Configuration

Navigate to **KKSR Faker** in WordPress admin menu.

### Ratings Tab
- **Auto Increment**: Enable/disable automatic rating increment
- **Cooldown Period**: Days before same visitor can rate again (default: 7)
- **Protection Threshold**: Stop at X ratings (default: 100)
- **Min Stars**: Minimum stars for fake votes (1-5, default: 4)
- **Max Stars**: Maximum stars for fake votes (1-5, default: 5)

### Sales Tab
- **Auto Increment**: Enable/disable automatic sales increment
- **Cooldown Period**: Days before same visitor counts again (default: 7)
- **Protection Threshold**: Stop at X sales (default: 50)

## 🎯 How It Works

### Ratings Flow
```
Unique Visitor → Post/Product Page
    ↓
Check: Not in session? Not in cooldown? Below threshold?
    ↓ YES
Random stars (4-5) → Update count & average
    ↓
Log visitor (hash + timestamp)
    ↓
Mark in session (prevents F5)
```

### Sales Flow
```
Unique Visitor → Product Page
    ↓
Check: Not in session? Not in cooldown? Below threshold?
    ↓ YES
+1 to total_sales
    ↓
Log visitor (hash + timestamp)
    ↓
Mark in session (prevents F5)
```

## 📊 Database

### Visitor Log Table
```sql
wp_kksr_visitor_log
├── visitor_hash (MD5)
├── object_id (post/product ID)
├── object_type ('post' or 'product')
├── data_type ('rating' or 'sales')
└── last_view_time (DATETIME)
```

### KKSR Meta Fields Updated
- `_kksr_count_default` - Rating count
- `_kksr_avg_default` - Average rating
- `_kksr_ratings_default` - Total stars
- `_kksr_casts` - Vote count
- `_kksr_avg` - Legacy average
- `_kksr_ratings` - Legacy total

### WooCommerce Meta
- `total_sales` - Product sales count

## 🔒 Privacy & Security

- **Hashed IDs**: Visitor identification uses MD5 hash, not raw IPs
- **No PII**: No personally identifiable information stored
- **Local Only**: All data stays on your server
- **Clean Uninstall**: Tracking data removed, real data preserved

## ⚠️ Important Notes

- **For Testing**: Recommended for development/testing environments
- **Compliance**: Check local laws regarding fake reviews/ratings
- **Transparency**: Consider disclosing to users if required
- **Real Data**: Plugin preserves and increments from existing numbers

## 🗑️ Uninstallation

### What Gets Deleted
- Plugin settings (`kksr_faker_settings`)
- Visitor log table (`wp_kksr_visitor_log`)
- Plugin cache

### What Stays
- All ratings data (`_kksr_*` meta fields)
- All sales data (`total_sales`)
- Real user data

## 🐛 Troubleshooting

### Ratings not increasing?
- Check Auto Increment is ON
- Verify threshold not reached
- Try different browser (different User Agent)
- Check 7-day cooldown hasn't blocked you

### Headers already sent error?
- Plugin now uses `init` hook for session
- Should be fixed in v4.0.0+

### Counting on archive pages?
- Plugin now checks `is_singular()`
- Only single post/product pages tracked

## 📝 Changelog

### 4.0.0 (2026-01-16)
- **MAJOR UPDATE**: Complete rewrite to traffic-based system
- Added: Visitor tracking with IP + UA hash
- Added: Configurable cooldown period
- Added: Session-based F5 prevention
- Added: Tabbed admin (Ratings | Sales)
- Added: Random stars with min/max
- Added: `is_singular()` check
- Fixed: Session management (init hook)
- Changed: No more random fake numbers
- Removed: Static generation

### 3.0.0 (2026-01-15)
- Settings API rewrite
- Admin panel added

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Open pull request

## 📄 License

GPL v2 or later - [https://www.gnu.org/licenses/gpl-2.0.html](https://www.gnu.org/licenses/gpl-2.0.html)

## 👤 Author

**MaiSyDat**  
Website: [https://hupuna.com](https://hupuna.com)  
GitHub: [@MaiSyDat](https://github.com/MaiSyDat)

## 🙏 Credits

- Built for WordPress & WooCommerce
- Compatible with KK Star Ratings plugin
- Uses WordPress coding standards

---

**⭐ If you find this plugin useful, please star the repository!**
