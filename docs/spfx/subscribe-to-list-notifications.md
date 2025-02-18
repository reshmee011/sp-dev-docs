---
title: Subscribe to list notifications
description: Get notified and respond to changes to files in SharePoint Document Libraries
ms.date: 04/27/2020
ms.prod: sharepoint
ms.localizationpriority: medium
---

# Subscribe to list notifications

Introduced in SharePoint Framework v1.7.0, you can subscribe to changes to files stored in a SharePoint Document Library. This allows you to respond to changes when they happen, without having to regularly poll the contents of the library.

## Prerequisites

To use the SharePoint Framework list subscription capability, you first need to install the **@microsoft/sp-list-subscription** npm package. You can do it using the following code snippet:

```console
npm install @microsoft/sp-list-subscription --save --save-exact
```

## Get notified of changes to documents stored in a SharePoint Document Library

To subscribe to changes to files stored in a SharePoint Document Library, create an instance of the `ListSubscriptionFactory` class. Next, call the `createSubscription()` method passing the ID of the Document Library to which you want to subscribe. If the Document Library is located in the current site, it's sufficient to specify just the library ID. If it's located in a different site or site collection, then you need to specify their IDs as well.

Following, is an example of a list subscription for a Document Library located in the current site:

```typescript
import { ListSubscriptionFactory, IListSubscription } from '@microsoft/sp-list-subscription';
import { Guid } from '@microsoft/sp-core-library';

export default class LatestDocumentsWebPart extends BaseClientSideWebPart<ILatestDocumentsWebPartProps> {
  private _listSubscriptionFactory: ListSubscriptionFactory;
  private _listSubscription: IListSubscription;

  private createListSubscription(): void {
    this._listSubscriptionFactory = new ListSubscriptionFactory(this);
    this._listSubscription = this._listSubscriptionFactory.createSubscription({
      listId: Guid.parse(this.properties.listId),
      callbacks: {
        notification: this._loadDocuments.bind(this)
      }
    });
  }

  private _loadDocuments(): void {
    // load documents here
  }

  // omitted for brevity
}
```

When creating a list subscription, using the `callbacks.notification` property, you have to specify the method that should be called, when a change has been detected. The method doesn't have any arguments, and for security reasons, you don't get notified what has been changed exactly. To get the latest contents of the Document Library, adhering to the configured permissions, you can use either the SharePoint REST APIs or the Microsoft Graph.

## Additional configuration

Depending on your solution, you might need to provide additional configuration when subscribing to list notifications.

### Connect to a site collection in a multi-geography tenancy

If you use SharePoint in a multi-geography tenancy, you need to provide the domain where the particular site collection lives in. You can do it, using the `domain` property, for example:

```typescript
this._listSubscriptionFactory.createSubscription({
  domain: this.properties.siteDomain,
  siteId: Guid.parse(this.properties.siteId),
  webId: Guid.parse(this.properties.webId),
  listId: Guid.parse(this.properties.listId),
  callbacks: {
    notification: this._loadDocuments.bind(this)
  }
});
```

### Get notified when the subscription has been established or canceled

In some cases, you might want to get notified when the component that you're building successfully subscribed to list notifications or if the connection has been broken. This would allow you to communicate the connection to the user through the UI.

The list subscription API, exposes two additional callbacks that you can implement to respond to the subscription status. The following code illustrates the case, where the component will be notified when the subscription has been established and when it was disconnected.

```typescript
private createListSubscription(): void {
  this._listSubscriptionFactory.createSubscription({
    domain: this.properties.siteDomain,
    siteId: Guid.parse(this.properties.siteId),
    webId: Guid.parse(this.properties.webId),
    listId: Guid.parse(this.properties.listId),
    callbacks: {
      notification: this._loadDocuments.bind(this),
      connect: this._subscriptionConnected.bind(this),
      disconnect: this._subscriptionDisconnected.bind(this)
    }
  });
}

private _subscriptionConnected(): void {
  // code to execute when the subscription to the list has been established
}

private _subscriptionDisconnected(reason: string): void {
  // code to execute when the connection to the list has been disconnected
}
```

The method for the `callbacks.disconnect` callback passes as an argument the reason why the connection has been disconnected. The possible values are `io server disconnect`, when the connection has been broken by the client or `io client disconnect` when the server closed the connection.

## Considerations

- all SharePoint Framework components can use the list subscription capabilities
- you can subscribe to events from libraries and lists
- there's a few seconds delay between the change and the notification being received by the component
- components can subscribe to changes in multiple Document Libraries
- the change notification doesn't pass any information about the added or changed document. To see what has changed, use the SharePoint REST API or Microsoft Graph

## See also

- [List subscriptions sample code](https://github.com/SharePoint/sp-dev-fx-webparts/tree/master/samples/react-realtime-documents)
