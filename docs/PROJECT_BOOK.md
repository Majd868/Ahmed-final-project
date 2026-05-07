# StorePilot Project Book

## 1. Project Overview
**Project Name:** StorePilot  
**Repository:** `Majd868/Ahmed-final-project`  
**Platform:** Android application  
**Language:** Java  
**Architecture Style:** MVVM with Repository pattern and Room database

StorePilot is a retail store management application designed to support day-to-day operations such as authentication, product inventory management, sales tracking, purchase tracking, seasonal planning, marketing metric monitoring, reporting, task management, and user administration. The application appears tailored for clothing or fashion retail based on seeded sample products such as dresses, jackets, and sneakers.

The system uses Android Activities and Fragments for the presentation layer, ViewModels for presentation logic, Repositories for data access abstraction, and a Room database for persistence.

## 2. Problem Statement
Retail stores often need a lightweight internal system to manage stock, monitor sales, record purchases, assign tasks, view basic reports, and track seasonal campaigns. Manual tracking creates inconsistencies, delays, and data loss risks. StorePilot addresses this by centralizing operational data in one Android application.

## 3. Objectives
- Provide secure user setup and login.
- Support multiple operational roles such as owner, manager, and employee.
- Manage products and inventory details.
- Record store sales and purchases.
- Track marketing performance through video metrics.
- Manage seasonal campaigns and alerts.
- Assign and monitor staff tasks.
- Provide dashboard summaries and reports for decision making.
- Control feature visibility based on user permissions.

## 4. System Scope
### Included Functional Scope
- Initial owner setup
- User login and session handling
- Dashboard summary
- Product listing, viewing, adding, and editing
- Sales history and sales creation
- Purchase history and purchase creation
- Task listing and task add/edit
- Season listing and season add/edit
- Video metrics list and add
- Reports screen for sales totals
- User management screen
- Demo data seeding

### Out of Scope / Not Evident in Current Code
- Remote API integration
- Cloud sync
- Payment gateway integration
- Barcode scanning
- Notifications service
- Advanced analytics dashboard charts

## 5. Technology Stack
- **Language:** Java
- **Framework:** Android SDK
- **Architecture:** MVVM + Repository Pattern
- **Persistence:** Room Database
- **UI Components:** Activities, Fragments, RecyclerView, BottomNavigationView, PopupMenu, FloatingActionButton, TabLayout, Chips
- **State/Reactive Layer:** LiveData, ViewModelProvider

## 6. High-Level Architecture
The application follows layered architecture:

1. **Presentation Layer**
   - Activities: app entry, setup, login, add/edit forms.
   - Fragments: dashboard, list screens, reports, administration.
2. **ViewModel Layer**
   - Bridges UI and repositories.
   - Exposes LiveData to observe database-driven updates.
3. **Repository Layer**
   - Encapsulates data access operations.
   - Delegates to DAO interfaces from Room.
4. **Data Layer**
   - Room entities such as User, Product, Sale, Purchase, Task, Season, and VideoMetric.
   - `AppDatabase` as central persistence manager.
5. **Core Services**
   - `SessionManager` for current logged-in user.
   - `PermissionManager` for role-based feature access.
   - `CryptoUtil` for password hashing and verification.

## 7. Main Modules
### 7.1 Authentication Module
Responsible for first-time setup, owner creation, and login.
- `SetupActivity`
- `LoginActivity`
- `AuthViewModel`
- `UserRepository`
- `CryptoUtil`
- `SessionManager`

### 7.2 Dashboard Module
Displays operational summary values such as season alerts, today sales, low stock count, and pending tasks.
- `DashboardFragment`
- `SeasonViewModel`
- `SaleViewModel`
- `ProductViewModel`
- `TaskViewModel`

### 7.3 Inventory Module
Supports listing products, viewing details, and add/edit operations.
- `ProductListFragment`
- `ProductDetailsFragment`
- `AddEditProductActivity`
- `ProductViewModel`
- `ProductRepository`

### 7.4 Sales Module
Supports viewing sales history and adding sale records.
- `SalesHistoryFragment`
- `AddSaleActivity`
- `SaleViewModel`
- `SaleRepository`

### 7.5 Purchase Module
Supports viewing purchases and adding purchase records.
- `PurchaseHistoryFragment`
- `AddPurchaseActivity`
- `PurchaseViewModel`
- `PurchaseRepository`

### 7.6 Task Management Module
Supports personal, team, and private task viewing with add/edit support.
- `TaskListFragment`
- `AddEditTaskActivity`
- `TaskViewModel`
- `TaskRepository`

### 7.7 Season Management Module
Supports tracking business seasons and alerts before season end.
- `SeasonListFragment`
- `AddEditSeasonActivity`
- `SeasonViewModel`
- `SeasonRepository`

### 7.8 Marketing Metrics Module
Tracks social/video engagement metrics.
- `VideoMetricsFragment`
- `AddMetricActivity`
- `VideoMetricViewModel`
- `VideoMetricRepository`

### 7.9 Reports Module
Provides sales totals by time range.
- `ReportsFragment`
- `ReportsViewModel`

### 7.10 Administration Module
Provides user listing and administration-related visibility.
- `UserManagementFragment`
- `UserRepository`

## 8. Core Data Model Summary
### User
Represents a system user with role-based access.
- id
- username
- passwordHash
- salt
- role
- createdAt

### Product
Represents a retail product.
- id
- name
- category
- size
- color
- quantity
- price
- costPrice
- imageUrl
- createdAt

### Sale
Represents a completed sale transaction.
- id
- productId
- quantity
- totalPrice
- saleDate
- soldBy
- notes

### Purchase
Represents stock procurement.
- id
- productId
- quantity
- totalCost
- purchaseDate
- purchasedBy
- supplier
- notes

### VideoMetric
Represents marketing performance data.
- id
- title
- platform
- views
- likes
- shares
- comments
- videoDate
- recordedBy

### Season
Represents a business or campaign season.
- id
- name
- startDate
- endDate
- alertDaysBeforeEnd
- isActive
- notes

### Task
Represents an operational task.
- id
- title
- description
- assignedTo
- createdBy
- status
- priority
- isPrivate
- dueDate
- createdAt

## 9. Functional Requirements
1. The system shall check whether an owner account exists at first launch.
2. The system shall allow creating an initial owner account.
3. The system shall allow users to log in using username and password.
4. The system shall hash passwords and verify them securely.
5. The system shall maintain a logged-in session.
6. The system shall show a dashboard after successful login.
7. The system shall allow users to browse products.
8. The system shall allow authorized users to add or edit products.
9. The system shall allow users to view product details.
10. The system shall display sales history.
11. The system shall allow creation of sales records.
12. The system shall display purchase history.
13. The system shall allow creation of purchase records.
14. The system shall display tasks by personal/team/private scope.
15. The system shall allow creating and editing tasks.
16. The system shall display season records.
17. The system shall allow creating and editing seasons.
18. The system shall show season alerts on the dashboard.
19. The system shall display video metrics.
20. The system shall allow adding marketing metrics.
21. The system shall show reports filtered by week, month, and year.
22. The system shall enforce feature visibility based on permissions.
23. The system shall support demo data seeding during setup.

## 10. Non-Functional Requirements
- **Usability:** The system should provide simple mobile screens for retail staff.
- **Performance:** Database operations should run asynchronously using `AppDatabase.dbExecutor`.
- **Security:** Passwords should not be stored in plain text.
- **Maintainability:** MVVM and repository separation should improve code organization.
- **Scalability:** Additional modules can be added around the same architecture.
- **Reliability:** Room persistence should maintain structured local data.

## 11. User Roles
Based on code comments and permission checks, the system supports roles such as:
- OWNER
- STORE_MANAGER
- SHIFT_MANAGER
- EMPLOYEE

Role permissions are centrally interpreted by `PermissionManager`, which controls visibility for products, purchases, marketing, reports, seasons, and admin features.

## 12. Main Application Flow
1. App opens.
2. If no owner exists, user is redirected to setup.
3. Owner account is created.
4. User logs in.
5. Session is stored in `SessionManager`.
6. `MainActivity` opens and loads dashboard by default.
7. User navigates through bottom navigation and more menu.
8. Each feature screen observes data through a ViewModel.
9. ViewModel requests data from repository.
10. Repository queries Room database through DAO.

## 13. Screen Inventory
### Activities
- `LoginActivity`
- `SetupActivity`
- `MainActivity`
- `AddEditProductActivity`
- `AddSaleActivity`
- `AddPurchaseActivity`
- `AddMetricActivity`
- `AddEditSeasonActivity`
- `AddEditTaskActivity`

### Fragments
- `DashboardFragment`
- `ProductListFragment`
- `ProductDetailsFragment`
- `SalesHistoryFragment`
- `PurchaseHistoryFragment`
- `TaskListFragment`
- `VideoMetricsFragment`
- `ReportsFragment`
- `SeasonListFragment`
- `UserManagementFragment`

## 14. Notable Design Observations
- The app is strongly local-first and database-centered.
- Navigation mixes Activities for forms and Fragments for feature browsing.
- The dashboard aggregates data from multiple modules.
- Permissions are checked in UI logic before exposing actions.
- Demo mode is a useful onboarding feature for showcasing the system quickly.

## 15. Suggested Future Enhancements
- Add edit/delete support consistently across all modules.
- Add chart visualizations in reports.
- Add search and filtering in list screens.
- Add image upload support for products.
- Add notification reminders for tasks and season alerts.
- Add export to PDF/Excel.
- Add cloud backup and multi-device sync.
- Add audit logs for admin actions.

## 16. Conclusion
StorePilot is a well-structured Android Java application for internal store operations management. Its architecture is suitable for academic presentation and practical prototype demonstration. The combination of authentication, inventory, transactions, task management, reporting, seasonal tracking, and permissions makes it a complete retail operations project.
