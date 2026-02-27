<script lang="ts">
  type DocumentWithViewTransition = Document & {
    startViewTransition?: (update: () => void) => { finished: Promise<void> };
  };

  type NotificationItem = {
    id: string;
    name: string;
    avatar: string;
    action: string;
    target?: string;
    targetType?: "post" | "group" | "picture";
    time: string;
    message?: string;
    image?: string;
    unread: boolean;
  };

  let notifications: NotificationItem[] = [
    {
      id: "mark-webber",
      name: "Mark Webber",
      avatar: "/images/avatar-mark-webber.webp",
      action: "reacted to your recent post",
      target: "My first tournament today!",
      targetType: "post",
      time: "1m ago",
      unread: true,
    },
    {
      id: "angela-gray",
      name: "Angela Gray",
      avatar: "/images/avatar-angela-gray.webp",
      action: "followed you",
      time: "5m ago",
      unread: true,
    },
    {
      id: "jacob-thompson",
      name: "Jacob Thompson",
      avatar: "/images/avatar-jacob-thompson.webp",
      action: "has joined your group",
      target: "Chess Club",
      targetType: "group",
      time: "1 day ago",
      unread: true,
    },
    {
      id: "rizky-hasanuddin",
      name: "Rizky Hasanuddin",
      avatar: "/images/avatar-rizky-hasanuddin.webp",
      action: "sent you a private message",
      time: "5 days ago",
      message:
        "Hello, thanks for setting up the Chess Club. I've been a member for a few weeks now and I'm already having lots of fun and improving my game.",
      unread: false,
    },
    {
      id: "kimberly-smith",
      name: "Kimberly Smith",
      avatar: "/images/avatar-kimberly-smith.webp",
      action: "commented on your picture",
      targetType: "picture",
      time: "1 week ago",
      image: "/images/image-chess.webp",
      unread: false,
    },
    {
      id: "nathan-peterson",
      name: "Nathan Peterson",
      avatar: "/images/avatar-nathan-peterson.webp",
      action: "reacted to your recent post",
      target: "5 end-game strategies to increase your win rate",
      targetType: "post",
      time: "2 weeks ago",
      unread: false,
    },
    {
      id: "anna-kim",
      name: "Anna Kim",
      avatar: "/images/avatar-anna-kim.webp",
      action: "left the group",
      target: "Chess Club",
      targetType: "group",
      time: "2 weeks ago",
      unread: false,
    },
  ];

  const profileHref = (id: string) => `/profile/${id}`;

  const targetHref = (notification: NotificationItem) => {
    if (!notification.target || !notification.targetType) {
      return "/";
    }

    if (notification.targetType === "group") {
      return `/groups/${notification.target.toLowerCase().replace(/\s+/g, "-")}`;
    }

    if (notification.targetType === "picture") {
      return "/gallery/chess";
    }

    return `/posts/${notification.id}`;
  };

  const markAllAsRead = () => {
    const applyUnreadUpdate = () => {
      notifications = notifications.map((notification) => ({
        ...notification,
        unread: false,
      }));
    };

    const doc = document as DocumentWithViewTransition;

    if (doc.startViewTransition) {
      doc.startViewTransition(applyUnreadUpdate);
      return;
    }

    applyUnreadUpdate();
  };

  $: unreadCount = notifications.filter((notification) => notification.unread).length;
</script>

<main class="notifications-page">
  <section class="notifications-page__content">
    <header class="notifications-page__header">
      <div>
        <h1 class="notifications-page__title">
          Notifications
          <span class="notifications-page__count" style="view-transition-name: notifications-count;">{unreadCount}</span>
        </h1>
      </div>
      <div>
        <button class="notifications-page__button" on:click={markAllAsRead}>
          Mark all as read
        </button>
      </div>
    </header>

    <ul class="notifications-list" aria-label="Notifications">
      {#each notifications as notification}
        
        <li class="notifications-list__card" class:active={notification.unread}>
          <div class="notifications-list__visual">
            <a href={profileHref(notification.id)} aria-label={`View ${notification.name}'s profile`}>
              <img src={notification.avatar} alt={notification.name} width="39px" height="39px"/>
            </a>
          </div>

          <div class="notifications-list__body">
            <p class="notifications-list__title">
                <strong>{notification.name}</strong>
              {notification.action}
              {#if notification.target}
                <a class="notifications-list__link notifications-list__link--target" href={targetHref(notification)}>
                  {notification.target}
                </a>
              {/if}
            </p>

            <time class="notifications-list__timestamp">{notification.time}</time>

            {#if notification.message}
              <a class="notifications-list__excerpt-link" href="/messages/rizky-hasanuddin">
                <blockquote class="notifications-list__excerpt">
                  {notification.message}
                </blockquote>
              </a>
            {/if}
          </div>

          {#if notification.image}
              <img class="notifications-list__preview" src={notification.image} alt="Comment preview" />
          {/if}
        </li>
     
      {/each}
    </ul>
  </section>
</main>


