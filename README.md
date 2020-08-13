<p align="center">
  <img src="docs/banner.svg" alt="NosFaltaUno Logo" />
</p>

# NosFaltaUno

## Description

This repository is one of the activities of the [Free Software Club from the University of Córdoba](https://uco.es/aulasoftwarelibre) and has only educational purposes.

"NosFaltaUno" is a notice board that allows you to find people with your same interests.

This repository contains the fake backend server of the application.

## Getting Started

First, run the development server:

```bash
npm run start
# or
yarn start
```

Open [http://localhost:5000](http://localhost:5000) with your browser to see the result.

NosFaltaUno is [AGPL3 licensed](LICENSE).


## API

### Categories

- GET /categories

        [
            {
                id: "uuid",
                title: "string",
                subcategories: [
                    {
                        id: "uuid"
                        title: "string"
                    }
                ]
            }
        ]

* POST /categories (logged) (admin)

        {
            id: "uuid",
            title: "string",
        }

* POST /categories/:id/subcategories (logged)

        {
            id: "uuid",
            title: "string",
        }

### Subscriptions

- GET /subscriptions (logged)

  Get logged user subscriptions

        [
            {
                category_id: "uuid",
                title: "string",
            }
        ]

- POST /subscriptions (logged)

  Subscribe logged user to category/subcategory

        {
            category_id: "uuid"
        }

  Note: category_id can be category or subcategory id

- DELETE /subscriptions/:category_id (logged)

  Unsubscribe logged user to category/subcategory

### Activities

- GET /activities

        [
            {
                id: "uuid",
                title: "string",
                description: "string",
                created_at: "date",
                closed_at: "date"|null,
                ends_at: "date"|null,
                status: "open|closed",
                category_id: "uuid",
                category_title: "string"
                subcategory_id: "uuid",
                subcategory_title: "string",
                owner_id: "uuid",
                owner_fullname: "string",
                owner_avatar: "string"|null
            }
        ]

* GET /activities/:id

  See `GET /activities`

* POST /activities (logged)

        {
            id: "uuid",
            title: "string",
            description: "string",
            contact_info: ["string"],
            category_id: "uuid",
            ends_at: "date"|null
        }

* PATCH /activities/:id (logged) (owner)

        {
            status: "closed"
        }

### Registrations

- GET /registrations?user_id=uuid (logged)
- GET /registrations?activity_id=uuid (logged)
- GET /registrations?activity_owner_id=uuid (logged)

        [
            {
                id: "uuid",
                activity_id: "uuid",
                activity_title: "string",
                activity_owner_id: "uuid",
                activity_owner_fullname_id: "string",
                activity_owner_avatar: "string"|null,
                user_id: "uuid",
                user_fullname: "string",
                user_avatar: "string"|null,
                status: "pending|accepted|rejected",
                contact_info: ["string"] | null,
                created_at: "date",
                updated_at: "date"
            }
        ]

  Note 1: Logged user receive only registrations with `user_id` or `activity_owner_id` equals to his id.
  Note 2: `contact_info` only appears if `status` is _accepted_.

- POST /registrations (logged)

        {
            id: "uuid"
            activity_id: "uuid"
        }

- PATCH /registrations/:id (logged) (owner)

        {
            status: "accepted|rejected"
        }
