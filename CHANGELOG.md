# 📔 บันทึกโครงการและการเปลี่ยนแปลง (Diary & Changelog)

บันทึกการทำงาน วิวัฒนาการ และการเปลี่ยนแปลงสำคัญของโปรเจกต์ **Mall Marketplace** (e-mall)

---

## [สิ่งที่ต้องทำต่อไป] - (Next Steps)
### แผนงาน
- [x] เริ่มระบบ Docker Environment (MySQL, Redis)
- [x] รันสคริปต์ปรับปรุงฐานข้อมูล (`migration_v1.sql`)
- [x] สร้างโค้ด MyBatis อัตโนมัติสำหรับตาราง `ums_shop`
- [x] สร้างระบบ Login/Register สำหรับผู้ขาย (Backend + Frontend)
- [ ] พัฒนาหน้าจัดการสินค้า (Product Management)
- [ ] พัฒนาหน้าจัดการคำสั่งซื้อ (Order Management)
- [ ] ทดสอบ API ครบถ้วน

---

## [2025-12-19 04:08] - 🎉 Seller Portal MVP พร้อมใช้งาน!
### 🚀 สิ่งที่เพิ่มเข้ามา (Added)

#### Backend (`mall-seller`)
- **UmsSellerController**: สร้าง REST API สำหรับ `/seller/register` และ `/seller/login`
- **UmsSellerServiceImpl**: Implement Logic การลงทะเบียนผู้ขาย (สร้าง `UmsAdmin` + `UmsShop`)
- **Login Logic**: สร้าง JWT Token และ `loadUserByUsername()` สำหรับ Spring Security
- **SellerUserDetails**: สร้าง UserDetails สำหรับ Spring Security
- **MallSecurityConfig**: กำหนดค่า Security และ UserDetailsService
- **SellerConfig**: แก้ไข Springfox Swagger Compatibility กับ Spring Boot 2.7+
- **GlobalCorsConfig**: เปิด CORS ให้ Frontend เรียก API ได้
- **MyBatisConfig**: สแกน Mapper จากทั้ง `mall-mbg` และ `mall-seller`

#### Frontend (`mall-seller-web`)
- **Login Page** (`views/login/index.vue`): หน้าเข้าสู่ระบบ
- **Register Page** (`views/login/register.vue`): หน้าลงทะเบียนร้านค้าใหม่
- **Home Page** (`views/home/index.vue`): หน้า Dashboard ผู้ขาย
- **API Client** (`utils/request.js`, `api/seller.js`): Axios wrapper สำหรับเรียก API
- **THB Currency Filter**: Global filter `currency()` แสดงค่าเงินบาท (฿)
- **ESLint Config**: เพิ่ม `.eslintrc.js` เพื่อให้ Build ผ่าน

### 🐛 แก้ไขบัก (Bug Fixes)
- **Lombok Issue**: แก้ปัญหา `cannot find symbol getUsername()` โดยเพิ่ม Getter/Setter แบบ Manual ใน `SellerRegisterParam`
- **Circular Dependency**: แยก `PasswordEncoder` ออกจาก `MallSecurityConfig` เพื่อแก้ Circular Reference
- **Missing Bean (JwtTokenUtil)**: เพิ่ม `scanBasePackages = "com.macro.mall"` ใน `@SpringBootApplication`
- **Bean Override (PasswordEncoder)**: ลบ Bean ซ้ำออกจาก `SellerConfig`
- **Swagger NPE**: เพิ่ม `spring.mvc.pathmatch.matching-strategy: ant_path_matcher` และ BeanPostProcessor Workaround
- **node-sass**: แทนที่ด้วย `sass` (Dart Sass) เพื่อแก้ปัญหา Python dependency
- **ESLint**: เพิ่ม `.eslintrc.js` configuration file
- **Duplicate UmsShopMapper**: ลบไฟล์ที่ซ้ำกันออกจาก `mall-mbg`

### ✅ สถานะปัจจุบัน (Current Status)
| Service | Port | Status |
|---------|------|--------|
| Backend (`mall-seller`) | 8086 | ✅ Running |
- **Frontend (`mall-seller-web`):**
    - [x] Create Login/Register pages
    - [x] Create Home Dashboard page
    - [x] Implement Global Currency Filter (THB)
    - [x] **New:** Implement Professional Admin Layout (Sidebar, Navbar)
    - [x] **New:** Create Menu Structure (Product, Order, Marketing, Finance)
    - [x] **New:** Localize UI to Thai Language
    - [x] **New:** Implement Login/Logout state management
สำเร็จ

### 📸 Screenshots
- หน้า Seller Center Dashboard แสดง Total Sales (฿ 0.00) และ Total Orders (0) สำเร็จ

---

## [2025-12-19] - เริ่มต้นโครงการและวางมาตรฐาน
### 🚀 สิ่งที่เพิ่มเข้ามา (Added)
- **สถาปัตยกรรม Marketplace**: ออกแบบระบบรองรับหลายร้านค้า (Multi-vendor Architecture)
- **Backend Module**: สร้างโมดูลใหม่ `mall-seller` (Spring Boot) สำหรับร้านค้าโดยเฉพาะ
- **Frontend Portal**: สร้างโปรเจกต์เว็บใหม่ `mall-seller-web` (Vue.js + ElementUI)
- **โครงสร้างโปรเจกต์**: รวมโมดูลใหม่เข้ากับ Repository หลัก `macrozheng/mall`

### 📝 เอกสารและมาตรฐาน (Documentation)
- **RULES.md**: กำหนดกฎการเขียนโค้ดของโปรเจกต์ (การแยกข้อมูลร้านค้า, การตั้งชื่อ, ความปลอดภัย)
- **CODE_REVIEW_STANDARDS.md**: เพิ่มมาตรฐานการตรวจโค้ดสำหรับบริบท Marketplace

### 💾 ฐานข้อมูล & เครื่องมือ (Database & Tools)
- **Environment**: ติดตั้งและรัน Docker Containers (MySQL 8.0, Redis, MinIO)
- **Maven**: ติดตั้ง Maven ผ่าน Homebrew เพื่อแก้ปัญหา Build Failure
- **MyBatis Generator (MBG)**: ปรับแต่ง `generatorConfig.xml` และแก้ไข Dependency (`mall-common`) จนสามารถ Gen Code ของตาราง `ums_shop` ได้สำเร็จ
- **Migration**: นำเข้าข้อมูล `mall.sql` และ `mall_marketplace_update.sql` เข้า MySQL เรียบร้อยแล้ว

### 🔧 DevOps
- **Git Repo**: อัปโหลดโครงสร้างเริ่มต้นขึ้น `analogte/e-mall`

---

> *บันทึกโดย AI Assistant (Antigravity)*
