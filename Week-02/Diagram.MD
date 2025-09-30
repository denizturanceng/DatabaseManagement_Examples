erDiagram

    USER {
        int USER_ID PK
        string email
        string name
        string surname
        string subscription_type
    }

    PROFILE {
        int PROFILE_ID PK
        int USER_ID FK
        string age_group
    }

    CONTENT {
        int CONTENT_ID PK
        string content_name
        int year
        int duration
        int GENRE_ID FK
    }

    GENRE {
        int GENRE_ID PK
        string genre_name
    }

    ACTOR {
        int ACTOR_ID PK
        string name
        string surname
    }

    WATCH_HISTORY {
        int HISTORY_ID PK
        int PROFILE_ID FK
        int CONTENT_ID FK
        date date
    }

    CONTENT_ACTORS {
        int CONTENT_ID FK
        int ACTOR_ID FK
    }

    USER_CONTENT_INTERACTIONS {
        int INTERACTION_ID PK
        int PROFILE_ID FK
        int CONTENT_ID FK
        string interaction_type
        date date
    }

    %% İlişkiler
    USER ||--o{ PROFILE : "has"
    PROFILE ||--o{ WATCH_HISTORY : "records"
    CONTENT ||--o{ WATCH_HISTORY : "watched"
    PROFILE ||--o{ USER_CONTENT_INTERACTIONS : "interacts"
    CONTENT ||--o{ USER_CONTENT_INTERACTIONS : "interacts"
    CONTENT ||--o{ CONTENT_ACTORS : "features"
    ACTOR ||--o{ CONTENT_ACTORS : "acts in"
    GENRE ||--o{ CONTENT : "categorizes"

