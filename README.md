# Підтвердження віку при отриманні посилки з поштомата
# Age Verification for Parcel Locker Delivery

🇺🇦 Українська | 🇬🇧 English

---

## 🇺🇦 Українська

### Про проєкт

Цей репозиторій містить бізнес- та інтеграційний аналіз кейсу доставки товарів із віковими обмеженнями через поштомати.

Мета рішення — змоделювати процес підтвердження віку користувача за допомогою **Дії** та запропонувати безпечний механізм перевірки одноразового коду перед відкриттям комірки поштомата.

### Учасники процесу

- **Company X** — Backend, мобільний застосунок та кур'єр;
- **Наша система** — Backend системи управління поштоматами та ПЗ поштомата;
- **Дія** — сервіс підтвердження користувача та його віку;
- **Користувач** — отримувач посилки.

### Реалізовано

У межах кейсу підготовлено:

- BPMN 2.0 діаграму процесу від створення посилки до її отримання;
- взаємодію між Company X, нашою системою, Дією та користувачем;
- механізм валідації одноразового коду підтвердження віку;
- схему інтеграції для перевірки коду;
- основні альтернативні та помилкові сценарії;
- assumptions та відкриті питання перед передачею функціоналу в розробку.

---

### BPMN 2.0

BPMN-модель описує основний сценарій для посилки, яка під час створення вже має ознаку необхідності перевірки віку.

Основний процес:

1. Company X створює посилку через API та передає ознаку необхідності перевірки віку.
2. Наша система створює посилку та повертає Courier Code і Customer Code.
3. Кур'єр доставляє посилку до поштомата.
4. Користувач вводить Customer Code.
5. Поштомат вимагає підтвердження віку.
6. Користувач запускає перевірку через мобільний застосунок Company X.
7. Company X взаємодіє з Дією.
8. Після успішного підтвердження користувач отримує одноразовий код.
9. Код вводиться на поштоматі та перевіряється нашою системою.
10. Валідний код дозволяє відкрити комірку.

BPMN-файл:

`docs/bpmn/age-verification-locker.bpmn`

---

### Запропонований механізм валідації

Рекомендовано механізм **попередньої реєстрації одноразового коду (Pre-registered OTP Validation)**.

Після успішного підтвердження віку:

1. Backend Company X генерує одноразовий код.
2. Company X реєструє код у Backend нашої системи для конкретної посилки.
3. Наша система зберігає захищене представлення коду, його статус та TTL.
4. Лише після успішної реєстрації Company X показує код користувачу.
5. Користувач вводить код на поштоматі.
6. ПЗ поштомата передає код нашому Backend.
7. Backend перевіряє код, TTL, статус та прив'язку до посилки.
8. Після успішної перевірки код стає використаним, а поштомат отримує дозвіл відкрити комірку.

### Чому обрано цей підхід

У момент фактичного отримання посилки поштомат залежить лише від Backend нашої системи.

Не потрібен додатковий синхронний запит:

`Поштомат → Наша система → Company X → Наша система → Поштомат`

Це:

- скорочує критичний інтеграційний ланцюжок;
- зменшує кількість точок відмови біля поштомата;
- спрощує контроль TTL та одноразовості OTP;
- дозволяє контролювати rate limiting та кількість невдалих спроб на стороні нашої системи.

Детальний опис:

`docs/uk/validation-mechanism.md`

---

### Edge cases та відкриті питання

Окремо розглядаються:

- неуспішне підтвердження віку;
- недоступність Дії або Company X;
- неможливість зареєструвати OTP;
- невалідний або прострочений код;
- повторне використання OTP;
- перевищення кількості невдалих спроб;
- повторна генерація коду;
- недоступність Backend нашої системи;
- offline-стан поштомата;
- одночасні запити на використання одного OTP;
- ситуація, коли код валідний, але комірка фізично не відкрилася.

Документація:

- `docs/uk/edge-cases.md`
- `docs/uk/assumptions-and-open-questions.md`

---

## 🇬🇧 English

### About the project

This repository contains the business and integration analysis of an age-restricted goods delivery flow using parcel lockers.

The objective is to model an age-verification process using **Diia** and propose a secure mechanism for validating a one-time code before a locker compartment is opened.

### Process participants

- **Company X** — backend, mobile application, and courier;
- **Our System** — parcel locker management backend and locker software;
- **Diia** — user and age verification service;
- **User** — parcel recipient.

### Deliverables

The case includes:

- a BPMN 2.0 process diagram covering the flow from parcel creation to collection;
- interaction between Company X, Our System, Diia, and the User;
- a proposed age-verification code validation mechanism;
- an integration validation schema;
- key alternative and error scenarios;
- assumptions and open questions to be clarified before development.

---

### BPMN 2.0

The BPMN model represents the main flow for a parcel that is created with an age-verification requirement.

Main flow:

1. Company X creates a parcel through the API and provides the age-verification flag.
2. Our System creates the parcel and returns the Courier Code and Customer Code.
3. The courier delivers the parcel to the locker.
4. The User enters the Customer Code.
5. The locker requests age verification.
6. The User starts age verification through the Company X mobile application.
7. Company X interacts with Diia.
8. After successful verification, the User receives a one-time code.
9. The code is entered at the locker and validated by Our System.
10. A valid code authorizes the locker compartment to open.

BPMN file:

`docs/bpmn/age-verification-locker.bpmn`

---

### Proposed validation mechanism

The recommended solution is **Pre-registered OTP Validation**.

After successful age verification:

1. Company X Backend generates a one-time code.
2. Company X registers the code with Our System Backend for the relevant parcel.
3. Our System stores a protected representation of the code together with its status and TTL.
4. Company X displays the code to the User only after successful registration.
5. The User enters the code at the parcel locker.
6. Locker software submits the code to Our System Backend.
7. The Backend validates the code, TTL, status, and parcel association.
8. After successful validation, the code is marked as used and the locker is authorized to open the compartment.

### Why this approach

At the moment of parcel collection, the locker depends only on Our System Backend.

It avoids the synchronous dependency chain:

`Locker → Our System → Company X → Our System → Locker`

This:

- reduces the number of failure points during parcel collection;
- shortens the critical integration path;
- allows Our System to control OTP lifetime and one-time usage;
- enables rate limiting and failed-attempt controls on our side.

Detailed documentation:

`docs/en/validation-mechanism.md`

---

### Edge cases and open questions

The solution separately considers:

- failed age verification;
- Diia or Company X unavailability;
- OTP registration failure;
- invalid or expired codes;
- repeated OTP usage;
- exceeded failed-attempt limits;
- OTP regeneration;
- Our System Backend unavailability;
- locker offline mode;
- concurrent attempts to use the same OTP;
- a valid OTP followed by a physical locker-opening failure.

Documentation:

- `docs/en/edge-cases.md`
- `docs/en/assumptions-and-open-questions.md`

---

## Repository structure

```text
age-verification-locker-integration/
├── README.md
├── LICENSE
├── docs/
│   ├── bpmn/
│   │   └── age-verification-locker.bpmn
│   ├── uk/
│   │   ├── validation-mechanism.md
│   │   ├── edge-cases.md
│   │   └── assumptions-and-open-questions.md
│   └── en/
│       ├── validation-mechanism.md
│       ├── edge-cases.md
│       └── assumptions-and-open-questions.md
└── images/
    ├── bpmn-process.png
    └── validation-schema.png
