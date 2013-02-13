---
layout: post
title: "HashBuilder"
date: 2013-02-12 17:48
comments: false
categories:
---

Creating custom data objects as subclasses of NSObject is something which tends to happen
quite often, and making those objects "good citizens" which can be reliably placed into
NS collections (`NSDictionary`, `NSSet`, `NSArray`, etc.) means taking a few extra steps.
One of these steps is to override `NSObject`'s `- (BOOL)isEqual:(id)object` method, and,
of course if you do that, you should (read: _must_) also override `- (NSUInteger)hash`.

Providing a good hash value from your overridden `- (NSUInteger)hash` method is a deep
topic, luckily we can stand on the backs of giants, and use knowledge and techniques
which follow "known good" practices. With that in mind, I created a utility, HashBuilder,
which makes generating a suitable hash value trivial.

[HashBuilder](https://github.com/levigroker/HashBuilder) can be used to build a hash
result from contributed objects or hashes (presumably properties on your object which
should be considered in the isEqual: override). The intention is for the hash result to be
returned from an override to the `NSObject` `- (NSUInteger)hash` method.

### Documentation

To use, create a HashBuilder object, contribute to it, then query the 'builtHash'
property for the resulting hash.

	- (NSUInteger)hash
	{
		HashBuilder *builder = [HashBuilder builder];

		[builder contributeObject:self.objectID];
		[builder contributeObject:self.occurredDate];
		[builder contributeObject:self.type];
		[builder contributeObject:self.objectURL];
		[builder contributeObject:self.tags];
		[builder contributeObject:self.count];

		NSUInteger retVal = builder.builtHash;

		return retVal;
	}

It is prudent to consider the same properties when overriding your `- (BOOL)isEqual:(id)object`
method as well.

NOTE: The order of contribution _will_ change the resulting hash, even if all
the same values are contributed. For example:

    HashBuilder *builder1 = [HashBuilder builder];
    [builder1 contributeObject:@"a"];
    [builder1 contributeObject:[NSNumber numberWithInteger:12345]];
    NSUInteger hash1 = builder1.builtHash;

    HashBuilder *builder2 = [HashBuilder builder];
    [builder2 contributeObject:[NSNumber numberWithInteger:12345]];
    [builder2 contributeObject:@"a"];
    NSUInteger hash2 = builder2.builtHash;

`hash1 != hash2`

### Installing

If you're using [CocoPods](http://cocopods.org) it's as simple as adding this to your `Podfile`:

	pod 'HashBuilder', '~> 1.0'

NOTE: HashBuilder makes use of techniques and concepts presented by [Mike Ash](http://www.mikeash.com/) in his post entitled [Implementing Equality and Hashing](http://www.mikeash.com/pyblog/friday-qa-2010-06-18-implementing-equality-and-hashing.html)
