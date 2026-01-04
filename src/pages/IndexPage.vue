<template>
  <q-page class="bg-white">
    <!-- Hero Section -->
    <div
      class="row items-center q-px-md q-py-xl bg-grey-1 relative-position overflow-hidden"
      style="min-height: 85vh"
    >
      <!-- Abstract Background Blob -->
      <div
        class="absolute-top-right gt-sm"
        style="right: -150px; top: -150px; opacity: 0.1; z-index: 0"
      >
        <q-icon name="cloud" color="primary" size="600px" />
      </div>

      <div
        class="container row items-center justify-between full-width q-mx-auto relative-position"
        style="max-width: 1200px; z-index: 1"
      >
        <div class="col-12 col-md-6 q-pr-md-xl">
          <q-badge
            color="blue-1"
            text-color="primary"
            class="q-py-xs q-px-sm q-mb-md text-weight-bold rounded-borders"
          >
            🚀 #1 CRM for Growing Teams
          </q-badge>
          <h1
            class="text-h2 text-weight-bolder text-dark q-mt-none q-mb-md"
            style="line-height: 1.1"
          >
            Manage Customers <br />
            <span class="text-primary">Without The Chaos.</span>
          </h1>
          <p class="text-h6 text-grey-7 q-mb-lg" style="max-width: 500px; line-height: 1.5">
            ClientFlow gives you a 360-degree view of your customers. Track leads, manage projects,
            and automate your workflow in one beautiful interface.
          </p>
          <div class="row q-gutter-md">
            <q-btn
              unelevated
              color="primary"
              size="lg"
              label="Start Free Trial"
              icon-right="arrow_forward"
              no-caps
              class="q-px-xl"
            />
            <q-btn
              outline
              color="primary"
              size="lg"
              label="Watch Demo"
              icon="play_circle"
              no-caps
              class="q-px-lg"
            />
          </div>
          <div class="row items-center q-mt-lg text-grey-6 text-caption">
            <q-icon name="verified" color="positive" class="q-mr-xs" size="16px" /> No credit card
            required &nbsp;&bull;&nbsp; 14-day free trial
          </div>
        </div>

        <div class="col-12 col-md-6 gt-sm relative-position text-center">
          <!-- Mockup Card -->
          <q-card
            class="shadow-24 my-card q-ml-auto"
            style="
              border-radius: 20px;
              overflow: hidden;
              transform: rotate(-2deg);
              border: 4px solid #fff;
              max-width: 500px;
            "
          >
            <q-img src="https://cdn.quasar.dev/img/material.png" :ratio="16 / 9" />

            <div class="q-pa-lg bg-white text-left">
              <div class="text-subtitle1 text-weight-bold q-mb-sm">Recent Activity</div>

              <div class="row items-center q-mb-md q-pa-sm bg-grey-1 rounded-borders">
                <q-avatar size="40px" color="blue-1" text-color="primary">JD</q-avatar>
                <div class="q-ml-md">
                  <div class="text-subtitle2 text-dark">John Doe</div>
                  <div class="text-caption text-grey">New Opportunity Created</div>
                </div>
                <q-space />
                <div class="text-caption text-primary text-weight-bold">$2,500</div>
              </div>

              <div class="row items-center q-pa-sm bg-grey-1 rounded-borders">
                <q-avatar size="40px" color="green-1" text-color="green">AS</q-avatar>
                <div class="q-ml-md">
                  <div class="text-subtitle2 text-dark">Sarah Smith</div>
                  <div class="text-caption text-grey">Ticket Resolved</div>
                </div>
                <q-space />
                <q-icon name="check_circle" color="positive" />
              </div>
            </div>
          </q-card>
        </div>
      </div>
    </div>

    <!-- Stats Section -->
    <div class="bg-dark text-white q-py-xl">
      <div class="container row justify-around text-center q-mx-auto" style="max-width: 1000px">
        <div v-for="stat in stats" :key="stat.label" class="col-12 col-sm-4 q-mb-md-none q-mb-lg">
          <div class="text-h3 text-weight-bold text-secondary">{{ stat.value }}</div>
          <div class="text-body1 text-grey-5 q-mt-sm">{{ stat.label }}</div>
        </div>
      </div>
    </div>

    <!-- Trusted By Section -->
    <div class="q-py-lg bg-white border-bottom q-px-md text-center">
      <div class="text-caption text-grey-6 text-uppercase q-mb-md">
        Trusted by 500+ innovative teams
      </div>
      <div class="row justify-center items-center q-gutter-xl opacity-60 grayscale filter">
        <div class="text-h6 text-grey-8 text-weight-bold">Acme Corp</div>
        <div class="text-h6 text-grey-8 text-weight-bold">GlobalTech</div>
        <div class="text-h6 text-grey-8 text-weight-bold">Nebula</div>
        <div class="text-h6 text-grey-8 text-weight-bold">FoxRun</div>
        <div class="text-h6 text-grey-8 text-weight-bold">Circle.io</div>
      </div>
    </div>

    <!-- Features Showcase Section -->
    <div class="q-py-xl bg-white q-px-md" id="features">
      <div class="container q-mx-auto" style="max-width: 1200px">
        <div class="text-center q-mb-xl">
          <div class="text-primary text-weight-bold text-uppercase tracking-wide q-mb-sm">
            Powerful Capabilities
          </div>
          <h2 class="text-h3 text-weight-bold text-dark q-ma-none">
            Everything you need to <span class="text-primary">grow better</span>
          </h2>
        </div>

        <q-card class="shadow-1 rounded-borders" style="border-radius: 16px; overflow: hidden">
          <q-tabs
            v-model="featuresTab"
            class="text-grey-7 bg-grey-1"
            active-color="primary"
            active-bg-color="white"
            indicator-color="primary"
            align="justify"
            narrow-indicator
          >
            <q-tab name="sales" label="Sales Pipeline" icon="monetization_on" class="q-py-md" />
            <q-tab name="marketing" label="Marketing Automation" icon="campaign" class="q-py-md" />
            <q-tab name="support" label="Customer Support" icon="support_agent" class="q-py-md" />
            <q-tab name="analytics" label="Deep Analytics" icon="insights" class="q-py-md" />
          </q-tabs>

          <q-separator />

          <q-tab-panels
            v-model="featuresTab"
            animated
            class="bg-white q-pa-lg"
            transition-prev="fade"
            transition-next="fade"
          >
            <q-tab-panel name="sales" class="q-pa-none">
              <div class="row items-center q-col-gutter-xl">
                <div class="col-12 col-md-5">
                  <h3 class="text-h4 text-weight-bold q-mb-md text-dark">
                    Close deals faster than ever.
                  </h3>
                  <p class="text-h6 text-grey-7 q-mb-lg" style="line-height: 1.6">
                    Visualize your sales process with a drag-and-drop pipeline. Automate follow-ups
                    and track performance.
                  </p>
                  <q-list class="q-mb-lg text-body1 text-grey-8">
                    <q-item dense>
                      <q-item-section avatar><q-icon name="check" color="green" /></q-item-section>
                      <q-item-section>Customizable Deal Stages</q-item-section>
                    </q-item>
                    <q-item dense>
                      <q-item-section avatar><q-icon name="check" color="green" /></q-item-section>
                      <q-item-section>Automatic Lead Scoring</q-item-section>
                    </q-item>
                    <q-item dense>
                      <q-item-section avatar><q-icon name="check" color="green" /></q-item-section>
                      <q-item-section>Activity Logging & Call Recording</q-item-section>
                    </q-item>
                  </q-list>
                  <q-btn
                    color="primary"
                    label="Explore Sales Hub"
                    outline
                    no-caps
                    icon-right="arrow_forward"
                  />
                </div>
                <div class="col-12 col-md-7">
                  <q-img
                    src="https://cdn.quasar.dev/img/parallax2.jpg"
                    class="rounded-borders shadow-10"
                    style="border: 1px solid #eee; height: 350px"
                  >
                    <div
                      class="absolute-bottom text-subtitle1 text-center bg-white text-dark q-py-sm"
                    >
                      Sales Pipeline Dashboard
                    </div>
                  </q-img>
                </div>
              </div>
            </q-tab-panel>

            <q-tab-panel name="marketing" class="q-pa-none">
              <div class="row items-center q-col-gutter-xl">
                <div class="col-12 col-md-5">
                  <h3 class="text-h4 text-weight-bold q-mb-md text-dark">
                    Nurture leads on autopilot.
                  </h3>
                  <p class="text-h6 text-grey-7 q-mb-lg" style="line-height: 1.6">
                    Create personalized email campaigns and landing pages. Segment your audience for
                    maximum engagement.
                  </p>
                  <q-chip color="orange-1" text-color="orange-9" icon="email"
                    >Email Marketing</q-chip
                  >
                  <q-chip color="blue-1" text-color="blue-9" icon="ads_click">Ad Management</q-chip>
                  <q-chip color="purple-1" text-color="purple-9" icon="auto_fix_high"
                    >Automation</q-chip
                  >
                </div>
                <div class="col-12 col-md-7">
                  <div
                    class="bg-grey-2 rounded-borders flex flex-center text-grey-5"
                    style="height: 350px; border: 2px dashed #ccc"
                  >
                    <div class="text-center">
                      <q-icon name="campaign" size="80px" />
                      <div class="text-h6">Marketing Automation Workflow UI</div>
                    </div>
                  </div>
                </div>
              </div>
            </q-tab-panel>

            <q-tab-panel name="support" class="q-pa-none">
              <!-- Similar structure for Support -->
              <div class="row items-center q-col-gutter-xl">
                <div class="col-12 col-md-5">
                  <h3 class="text-h4 text-weight-bold q-mb-md text-dark">
                    Delight your customers 24/7.
                  </h3>
                  <p class="text-h6 text-grey-7 q-mb-lg" style="line-height: 1.6">
                    A unified inbox for all channels. Chatbots, knowledge base, and ticketing system
                    in one place.
                  </p>
                </div>
                <div class="col-12 col-md-7">
                  <div
                    class="bg-grey-2 rounded-borders flex flex-center text-grey-5"
                    style="height: 350px; border: 2px dashed #ccc"
                  >
                    <div class="text-center">
                      <q-icon name="support_agent" size="80px" />
                      <div class="text-h6">Support Ticket Inteface</div>
                    </div>
                  </div>
                </div>
              </div>
            </q-tab-panel>

            <q-tab-panel name="analytics" class="q-pa-none">
              <!-- Similar structure for Analytics -->
              <div class="row items-center q-col-gutter-xl">
                <div class="col-12 col-md-5">
                  <h3 class="text-h4 text-weight-bold q-mb-md text-dark">Data-driven decisions.</h3>
                  <p class="text-h6 text-grey-7 q-mb-lg" style="line-height: 1.6">
                    Real-time reporting on meaningful metrics. Forecast revenue and measure team
                    performance.
                  </p>
                </div>
                <div class="col-12 col-md-7">
                  <div
                    class="bg-grey-2 rounded-borders flex flex-center text-grey-5"
                    style="height: 350px; border: 2px dashed #ccc"
                  >
                    <div class="text-center">
                      <q-icon name="insights" size="80px" />
                      <div class="text-h6">Reporting Dashboard Mockup</div>
                    </div>
                  </div>
                </div>
              </div>
            </q-tab-panel>
          </q-tab-panels>
        </q-card>
      </div>
    </div>

    <!-- Pricing Section -->
    <div class="q-py-xl bg-grey-1 q-px-md">
      <div class="container q-mx-auto" style="max-width: 1200px">
        <div class="text-center q-mb-xl">
          <h2 class="text-h3 text-weight-bold text-dark q-mb-md">Simple, transparent pricing</h2>
          <p class="text-h6 text-grey-6">Choose the plan that's right for your business.</p>
        </div>

        <div class="row q-col-gutter-lg justify-center">
          <!-- Free Plan -->
          <div class="col-12 col-md-4">
            <q-card
              class="no-shadow q-pa-lg full-height column"
              style="border-radius: 16px; border: 1px solid #ddd"
            >
              <div class="q-mb-lg">
                <div class="text-h6 text-weight-bold text-dark">Starter</div>
                <div class="text-grey-7 q-mb-md">For small teams just getting started.</div>
                <div class="text-h3 text-weight-bold text-dark">
                  $0<span class="text-body1 text-grey-5">/mo</span>
                </div>
              </div>
              <q-list dense class="q-mb-xl col-grow">
                <q-item v-for="n in 3" :key="n" class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Up to 2 users</q-item-section>
                </q-item>
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Basic Deal Tracking</q-item-section>
                </q-item>
              </q-list>
              <q-btn outline color="primary" label="Get Started Free" class="full-width" no-caps />
            </q-card>
          </div>

          <!-- Pro Plan -->
          <div class="col-12 col-md-4">
            <q-card
              class="shadow-10 q-pa-lg full-height column relative-position"
              style="border-radius: 16px; border: 2px solid var(--q-primary)"
            >
              <div class="absolute-top text-center">
                <q-badge color="primary" class="q-py-xs q-px-md" style="margin-top: -12px"
                  >MOST POPULAR</q-badge
                >
              </div>
              <div class="q-mb-lg">
                <div class="text-h6 text-weight-bold text-dark">Professional</div>
                <div class="text-grey-7 q-mb-md">For growing teams needing more power.</div>
                <div class="text-h3 text-weight-bold text-dark">
                  $49<span class="text-body1 text-grey-5">/mo</span>
                </div>
              </div>
              <q-list dense class="q-mb-xl col-grow">
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Everything in Starter</q-item-section>
                </q-item>
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Up to 10 users</q-item-section>
                </q-item>
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Advanced Reporting</q-item-section>
                </q-item>
              </q-list>
              <q-btn color="primary" label="Start 14-Day Free Trial" class="full-width" no-caps />
            </q-card>
          </div>

          <!-- Enterprise Plan -->
          <div class="col-12 col-md-4">
            <q-card
              class="no-shadow q-pa-lg full-height column"
              style="border-radius: 16px; border: 1px solid #ddd"
            >
              <div class="q-mb-lg">
                <div class="text-h6 text-weight-bold text-dark">Enterprise</div>
                <div class="text-grey-7 q-mb-md">Custom solutions for large organizations.</div>
                <div class="text-h3 text-weight-bold text-dark">Custom</div>
              </div>
              <q-list dense class="q-mb-xl col-grow">
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Unlimited users</q-item-section>
                </q-item>
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Dedicated account manager</q-item-section>
                </q-item>
                <q-item class="q-px-none">
                  <q-item-section avatar
                    ><q-icon name="check_circle" color="primary"
                  /></q-item-section>
                  <q-item-section>Custom API access</q-item-section>
                </q-item>
              </q-list>
              <q-btn outline color="dark" label="Contact Sales" class="full-width" no-caps />
            </q-card>
          </div>
        </div>
      </div>
    </div>

    <!-- Testimonial Section -->
    <div class="q-py-xl bg-blue-1 q-px-md">
      <div class="container q-mx-auto text-center" style="max-width: 800px">
        <q-icon name="format_quote" size="60px" color="primary" class="q-mb-md opacity-50" />
        <h3 class="text-h4 text-weight-medium text-dark q-mb-lg" style="line-height: 1.4">
          "Since switching to ClientFlow, our retention rates have increased by 40%. It's strictly
          the most intuitive CRM we've ever used."
        </h3>
        <div class="text-h6 text-weight-bold">Emily Johnson</div>
        <div class="text-grey-7">VP of Sales, TechCorp</div>
      </div>
    </div>

    <!-- CTA Section -->
    <div class="bg-primary text-white q-py-xl q-px-md text-center">
      <div class="container q-mx-auto" style="max-width: 800px">
        <h2 class="text-h3 text-weight-bold q-mt-none q-mb-md">
          Ready to transform your business?
        </h2>
        <p class="text-h6 text-blue-2 q-mb-xl">
          Join 10,000+ companies managing their customers better.
        </p>
        <q-btn
          unelevated
          color="white"
          text-color="primary"
          size="xl"
          label="Get Started Now"
          no-caps
          class="q-px-xl text-weight-bold"
        />
      </div>
    </div>

    <!-- Footer -->
    <div class="bg-dark text-grey-5 q-py-lg q-px-md">
      <div class="container q-mx-auto row justify-between items-center" style="max-width: 1200px">
        <div class="text-subtitle1 text-white text-weight-bold">
          Client<span class="text-primary">Flow</span>
        </div>
        <div class="q-gutter-md">
          <a href="#" class="text-grey-5 text-decoration-none">Privacy</a>
          <a href="#" class="text-grey-5 text-decoration-none">Terms</a>
          <a href="#" class="text-grey-5 text-decoration-none">Contact</a>
        </div>
        <div>&copy; 2024 ClientFlow Inc.</div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'

const featuresTab = ref('sales')

const stats = [
  { value: '10k+', label: 'Active Business Users' },
  { value: '98%', label: 'Customer Satisfaction Score' },
  { value: '24/7', label: 'Global Support & Uptime' },
]
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
}
.hover-effect {
  transition: all 0.3s ease;
}
.hover-effect:hover {
  transform: translateY(-8px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.08) !important;
  border-color: var(--q-primary) !important;
}
.text-decoration-none {
  text-decoration: none;
}
.grayscale {
  filter: grayscale(100%);
  transition: all 0.3s ease;
}
.grayscale:hover {
  filter: grayscale(0%);
  opacity: 1;
}
.opacity-60 {
  opacity: 0.6;
}
</style>
