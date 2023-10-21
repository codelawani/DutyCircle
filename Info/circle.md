An example of how a simple circle could work:

1. A user creates a circle and becomes the admin of that circle.
2. The user invites other users to join the circle by sending them a circle invite.
3. The other users receive the invite and can choose to accept or decline it.
4. If a user accepts the invite, they become a member of the circle and can see the tasks of other members.
5. Members can create tasks and assign them to the circle, and other members can see and edit those tasks.

Here's an updated version of the `User` and `Circle` models that includes the necessary fields to implement circles:

```prisma
model User {
  id                    String         @id @default(uuid())
  createdAt             DateTime       @default(now())
  updatedAt             DateTime       @updatedAt
  email                 String         @unique
  username              String         @unique
  password              String
  role                  Role           @default(USER)
  circleInvitesReceived CircleInvite[] @relation("CircleInvitesReceived")
  notifications         Notification[]
  tasks                 Task[]
  circles               Circle[]       @relation("Members", references: [id])
}

model Circle {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  name      String   @default("My Circle")
  admin     User     @relation("Admin", fields: [adminId], references: [id])
  adminId   String?
  members   User[]   @relation("Members", references: [id])
  tasks     Task[]
}

model CircleInvite {
  id          String   @id @default(uuid())
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  sender      User     @relation(fields: [senderId], references: [id])
  senderId    String
  recipient   User     @relation(fields: [recipientId], references: [id])
  recipientId String
  circle      Circle   @relation(fields: [circleId], references: [id])
  circleId    String
  status      InviteStatus
}

enum InviteStatus {
  PENDING
  ACCEPTED
  DECLINED
}
```

In this updated version of the `User` and `Circle` models, the `User` model has a new field called `circles`, which is a list of circles that the user is a member of. The `Circle` model has a new field called `members`, which is a list of users who are members of the circle.

To create a new circle, a user can send a POST request to the `/circles` endpoint with the circle name and a list of user IDs to invite. The backend can then create a new `Circle` object and a new `CircleInvite` object for each user who was invited.

When a user accepts a circle invite, the backend can add the user to the `members` list of the circle, and the user can see the tasks of other members. When a user creates a new task, they can assign it to the circle, and other members can see that task.

