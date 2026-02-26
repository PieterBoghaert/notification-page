<script lang="ts">
  type NotificationItem = {
    name: string;
    avatar: string;
    action: string;
    target?: string;
    time: string;
    message?: string;
    image?: string;
    unread: boolean;
  };

  let notifications: NotificationItem[] = [
    {
      name: "Mark Webber",
      avatar: "/images/avatar-mark-webber.webp",
      action: "reacted to your recent post",
      target: "My first tournament today!",
      time: "1m ago",
      unread: true,
    },
    {
      name: "Angela Gray",
      avatar: "/images/avatar-angela-gray.webp",
      action: "followed you",
      time: "5m ago",
      unread: true,
    },
    {
      name: "Jacob Thompson",
      avatar: "/images/avatar-jacob-thompson.webp",
      action: "has joined your group",
      target: "Chess Club",
      time: "1 day ago",
      unread: true,
    },
    {
      name: "Rizky Hasanuddin",
      avatar: "/images/avatar-rizky-hasanuddin.webp",
      action: "sent you a private message",
      time: "5 days ago",
      message:
        "Hello, thanks for setting up the Chess Club. I've been a member for a few weeks now and I'm already having lots of fun and improving my game.",
      unread: false,
    },
    {
      name: "Kimberly Smith",
      avatar: "/images/avatar-kimberly-smith.webp",
      action: "commented on your picture",
      time: "1 week ago",
      image: "/images/image-chess.webp",
      unread: false,
    },
    {
      name: "Nathan Peterson",
      avatar: "/images/avatar-nathan-peterson.webp",
      action: "reacted to your recent post",
      target: "5 end-game strategies to increase your win rate",
      time: "2 weeks ago",
      unread: false,
    },
    {
      name: "Anna Kim",
      avatar: "/images/avatar-anna-kim.webp",
      action: "left the group",
      target: "Chess Club",
      time: "2 weeks ago",
      unread: false,
    },
  ];

  const markAllAsRead = () => {
    notifications = notifications.map((notification) => ({
      ...notification,
      unread: false,
    }));
  };

  $: unreadCount = notifications.filter((notification) => notification.unread).length;
</script>

<main class="notifications-page">
  <section class="notifications-page__content">
    <header class="notifications-page__header">
      <div>
        <h1 class="notifications-page__title">
          Notifications
          <span class="notifications-page__count">{unreadCount}</span>
        </h1>
      </div>
      <div>
        <button class="notifications-page__button" on:click={markAllAsRead}>
          Mark all as read
        </button>
      </div>
    </header>

    <div class="notifications-list">
      {#each notifications as notification}
        <article class="notifications-list__card" class:active={notification.unread}>
          <div class="notifications-list__visual">
            <img src={notification.avatar} alt={notification.name} width="39px" height="39px"/>
          </div>

          <div class="notifications-list__body">
            <p class="notifications-list__title">
              <strong>{notification.name}</strong>
              {notification.action}
              {#if notification.target}
                <strong>{notification.target}</strong>
              {/if}
            </p>

            <time class="notifications-list__timestamp">{notification.time}</time>

            {#if notification.message}
              <blockquote class="notifications-list__excerpt">
                {notification.message}
              </blockquote>
            {/if}
          </div>

          {#if notification.image}
            <img class="notifications-list__preview" src={notification.image} alt="Comment preview" />
          {/if}
        </article>
      {/each}
    </div>
  </section>
</main>


