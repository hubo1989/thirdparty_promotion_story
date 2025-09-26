# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Joint Marketing Platform Third-Party Marketing Management - Yunshuhui Integration Project** (联合营销平台三方营销管理-云数汇接入项目). It's a requirement documentation repository for building a multi-channel coupon marketing platform that integrates with Yunshuhui third-party coupon services.

## System Architecture

The platform consists of three interconnected systems:

- **Theme Activity Management System**: Handles marketing campaign planning, configuration, and management including lottery wheel interactions
- **Activity Coupon Management System**: Manages multi-channel coupon campaigns (WeChat/Alipay/UnionPay) and links to theme activities
- **Yunshuhui Integration Layer**: Wraps Yunshuhui APIs to provide unified coupon distribution services

### Core Architecture Flow

```
User → Bank System → Joint Marketing Platform → Yunshuhui → WeChat Platform
Each layer handles: Authorization → Coupon Distribution → Redemption → Data Sync
```

## Development Commands

### Static Documentation Development
```bash
# No build step required - this is a documentation repository
# All files are static HTML/CSS/JavaScript documentation

# View main documentation
open index.html

# View specific system documentation
open activity_story.html     # Theme Activity Management System requirements
open promotion_story.html    # Activity Coupon Management System requirements

# View prototypes
open activity_prototype.html   # Theme activity management mockups
open promotion_prototype.html  # Coupon management interface mockups

# View API documentation
open api/接口文档.md
```

### Deployment Commands
```bash
# Current deployment is via Vercel static hosting
# Configuration in .vercel/project.json
# Domain configured via CNAME: ysh.20160325.xyz

# Manual deployment steps (if needed)
vercel --prod                    # Deploy to production
```

### Documentation Preview & Testing
```bash
# Serve documentation locally (no Node.js required)
python3 -m http.server 8000     # Simple HTTP server
# Or use any static file server

# Validate HTML documents
html-validate *.html           # If html-validate is available

# Check responsive design
# Open chrome devtools → device toolbar → test mobile/tablet layouts
```

## File Structure

```
thirdparty_promotion_story/
├── index.html                     # Main requirements document
├── README.md                      # Project overview and navigation
├── CNAME                          # Custom domain for Vercel
├── activity_story.html            # Theme Activity System - complete requirements
├── activity_prototype.html        # Theme Activity System - UI prototypes
├── promotion_story.html           # Coupon Activity System - complete requirements  
├── promotion_prototype.html       # Coupon Activity System - UI prototypes
├── IFLOW.md                       # iFlow CLI context documentation
├── api/
│   └── 接口文档.md                # Yunshuhui API technical specifications
└── image/
    ├── activity_preview.png       # Theme activity system preview screenshots
    └── promotion_preview.png      # Coupon management system preview screenshots
```

## Yunshuhui API Integration

### Core API Endpoints

1. **Get OpenID**: Embedded H5 URL for WeChat auth
   - Test: `https://bmk-market.dshytest.com/deliverWM/1017578611540230144?wmcb=[callback_url]`
   - Prod: `https://market.dsaika.com/deliverWM/1019393703635943424?wmcb=[callback_url]`

2. **Coupon Distribution**: POST endpoint with SHA256+RBA signature
   - Uses: member_id, activity_id, owner_id, promotion_id, channel_openid

3. **Distribution Callback**: Retry mechanism with exponential backoff (1s→2h→4h total)

4. **Redemption Callback**: Parallel notification flow for coupon usage

### Security Requirements
- **Authentication**: LKLAPI-SHA256withRSA signature mechanism
- **Encryption**: HTTPS/TLS required for all endpoints
- **Retry Logic**: 4h3m21s retry schedule with SUCCESS response termination

## Technical Specifications

### Frontend
- **Technology**: Static HTML5, CSS3, ES6+ JavaScript
- **Styling**: Responsive design targeting desktop/tablet
- **Structure**: Word-compatible layout with print styles
- **Navigation**: Automatic table of contents generation via JavaScript

### Documentation Standards
- **Language**: Simplified Chinese with technical terminology
- **Format**: HTML with embedded SVG diagrams for sequence flows
- **Fonts**: Songti/宋体 for Chinese text, system fonts for English
- **Print**: @media print rules for paper documentation
- **Structure**: Section-anchored navigation for large documents

### Development Tools & Environment
```bash
# Web Server for local development
# Built-in tools - no dependencies required
python3 -m http.server 8000  # Python 3
# Or use:  npx serve .  # If Node.js available

# Browser Testing
# Support matrix: Chrome 70+, Firefox 65+, Safari 12+, Edge 79+
```

## Key Business Concepts

### Theme Activity System
- High-level campaign container
- Configures lottery wheels, prize pools, rules
- Links to specific coupon campaigns
- Manages campaign lifecycle: draft→active→paused→ended

### Activity Coupon System  
- Channel-specific coupon campaigns (WeChat/Alipay/UnionPay)
- Links to parent theme activity via activity_id
- Handles coupon lifecycle: creation→distribution→redemption→reporting
- Tracks ROI metrics and redemption rates

### Integration Points
- **Data synchronization**: Real-time coupon status updates
- **Security**: Bank-level data isolation by institution
- **Reporting**: Unified analytics across both systems
- **Operations**: Single management portal for bank staff

## Development Patterns

### Document Structure Pattern
Each system has paired files: `[system]_story.html` (detailed requirements) + `[system]_prototype.html` (UI mockups)

### Navigation Architecture
- Cross-file linking between related documents
- JavaScript-powered interactive navigation
- Responsive sidebar for large documents
- Print-friendly layouts for offline reading

### API Documentation Style
- Tabular parameter specifications
- JSON request/response examples
- Endpoint-specific authentication notes
- Error code reference tables

## Version Control

This project uses Git with Vercel for deployment:
- **Main branch**: `main`  
- **Deployment trigger**: Auto-deploy on push to main
- **Domain**: ysh.20160325.xyz (configured via CNAME)
- **Vercel config**: .vercel/project.json with project:true映射