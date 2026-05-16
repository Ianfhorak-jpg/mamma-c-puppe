---
name: html-advertorial
description: Build high-converting HTML advertorials, native ads, and sales landing pages. Use when creating sponsored content articles, listicle-style ads, product advertorials, or any page designed to look editorial but convert to a sale. Based on the Veloura / Luis_Website project style.
argument-hint: [product] [target-audience]
---

# HTML Advertorial Builder

You are an expert in high-converting advertorial design and copywriting. Build persuasive HTML pages that look editorial, read naturally, and convert readers into buyers.

## What Is an Advertorial?

An advertorial looks like a news article or blog post but is actually a paid ad. It:
- Reads like editorial journalism (not a sales pitch)
- Tells a story (problem → discovery → solution)
- Has a clear CTA at the end
- Passes as "native" content on ad networks (Taboola, Outbrain)

## Advertorial Structure

```
1. HEADLINE      — Curiosity hook, not product name
2. SUBHEADLINE   — Expand the hook, hint at the solution
3. STORY OPENER  — Relatable problem (first person or third person)
4. AGITATION     — Make the problem feel urgent/painful
5. DISCOVERY     — How the product/solution was found
6. PROOF         — Social proof, statistics, expert quotes
7. PRODUCT REVEAL— Natural introduction of the product
8. BENEFITS LIST — 3-5 bullets, outcomes not features
9. TESTIMONIALS  — 2-3 specific, detailed reviews
10. CTA          — Single clear call to action button
```

## HTML Template (Veloura-Style)

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ich habe es endlich gefunden – nach Jahren des Suchens</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: Georgia, serif; color: #222; background: #fff; }
    
    .article-header {
      max-width: 700px; margin: 0 auto; padding: 40px 20px 20px;
      border-bottom: 2px solid #c00; margin-bottom: 30px;
    }
    .site-name { font-size: 12px; color: #666; text-transform: uppercase; letter-spacing: 2px; }
    h1 { font-size: 32px; line-height: 1.3; margin: 15px 0 10px; font-weight: bold; }
    .subheadline { font-size: 18px; color: #555; line-height: 1.5; }
    
    .article-body { max-width: 700px; margin: 0 auto; padding: 0 20px 60px; }
    p { font-size: 17px; line-height: 1.8; margin-bottom: 20px; }
    
    .highlight-box {
      background: #fff8e1; border-left: 4px solid #f5a623;
      padding: 20px 25px; margin: 30px 0; border-radius: 4px;
    }
    
    .testimonial {
      background: #f9f9f9; border-radius: 8px;
      padding: 20px 25px; margin: 25px 0;
    }
    .testimonial .stars { color: #f5a623; font-size: 20px; }
    .testimonial .name { font-size: 13px; color: #666; margin-top: 8px; }
    
    .cta-section { text-align: center; padding: 40px 20px; background: #f5f5f5; border-radius: 12px; margin-top: 40px; }
    .cta-btn {
      display: inline-block; background: #c00; color: #fff;
      padding: 18px 48px; font-size: 18px; font-weight: bold;
      border-radius: 6px; text-decoration: none;
      box-shadow: 0 4px 15px rgba(200,0,0,0.3);
    }
    .cta-btn:hover { background: #a00; }
    .urgency { color: #c00; font-size: 14px; margin-top: 12px; }
    
    @media (max-width: 600px) {
      h1 { font-size: 24px; }
      p { font-size: 16px; }
    }
  </style>
</head>
<body>

<div class="article-header">
  <div class="site-name">Gesundheit & Wellness | Gesponsert</div>
  <h1>HEADLINE HIER</h1>
  <p class="subheadline">SUBHEADLINE HIER</p>
</div>

<div class="article-body">
  <p>STORY OPENER — Start with "Ich" or a relatable character...</p>
  
  <p>AGITATION — Describe the problem in vivid detail...</p>

  <div class="highlight-box">
    <strong>Das Wichtigste zuerst:</strong> KEY INSIGHT OR STATISTIC HERE
  </div>
  
  <p>DISCOVERY — How they found the solution...</p>
  
  <p>PRODUCT REVEAL — Natural introduction...</p>
  
  <h2>Warum [PRODUCT] so gut funktioniert:</h2>
  <ul>
    <li>✅ BENEFIT 1 (outcome, not feature)</li>
    <li>✅ BENEFIT 2</li>
    <li>✅ BENEFIT 3</li>
  </ul>

  <div class="testimonial">
    <div class="stars">★★★★★</div>
    <p>"TESTIMONIAL TEXT HERE — specific, detailed, believable"</p>
    <div class="name">— Name, Age, City</div>
  </div>

  <div class="cta-section">
    <h2>Jetzt [PRODUCT] testen</h2>
    <p>RISK REVERSAL — Geld-zurück-Garantie, kostenloser Versand, etc.</p>
    <a href="AFFILIATE_LINK" class="cta-btn">Jetzt bestellen →</a>
    <p class="urgency">⚠️ Nur noch 47 Stück verfügbar</p>
  </div>
</div>

</body>
</html>
```

## Copywriting Rules

- **Headline formula**: `[Emotional trigger] + [Specific result] + [Time frame]`  
  Example: "Ich habe meine Migräne in 14 Tagen besiegt — ohne Tabletten"
- Never mention the product name in the headline
- First paragraph must be a story, never a product pitch
- Every paragraph should end making the reader want to read the next
- Use "Sie" for formal (health products, 40+) or "du" for young audience

## Instructions

1. Ask: product name, target audience, main pain point, language (DE/EN/AT)
2. Ask: What result does the customer want? (not what the product does)
3. Generate full HTML with complete copy — no Lorem Ipsum, real persuasive text
4. Include mobile-responsive CSS
5. Reference the Veloura project structure if user wants a multi-page package
