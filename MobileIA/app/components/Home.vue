
<template>
  <Frame>
    <Page backgroundColor="#F4F7FC">
      <ActionBar
        title="RévisIA"
        backgroundColor="#2563EB"
        color="#FFFFFF"
      />

      <ScrollView>
        <StackLayout padding="20">

          <!-- Présentation -->
          <Label
            text="Assistant de révision intelligent"
            fontSize="22"
            fontWeight="bold"
            color="#1E3A8A"
            textWrap="true"
            marginBottom="5"
          />

          <Label
            text="Transformez vos cours en fiches de révision grâce à l'intelligence artificielle."
            fontSize="14"
            color="#475569"
            textWrap="true"
            marginBottom="20"
          />

          <!-- Texte -->
          <Label
            text="Texte à réviser"
            fontSize="16"
            fontWeight="bold"
            color="#334155"
            marginBottom="8"
          />

          <TextView
            v-model="texte"
            hint="Collez ici votre texte de cours..."
            height="180"
            backgroundColor="#FFFFFF"
            color="#1E293B"
            fontSize="15"
            padding="12"
            marginBottom="15"
          />

          <!-- Modes -->
          <Label
            text="Type de révision"
            fontSize="16"
            fontWeight="bold"
            color="#334155"
            marginBottom="10"
          />

          <Button
            text="Résumé"
            @tap="typeRevision = 'resume'"
            :backgroundColor="typeRevision === 'resume' ? '#2563EB' : '#DBEAFE'"
            :color="typeRevision === 'resume' ? '#FFFFFF' : '#1E40AF'"
            marginBottom="5"
          />

          <Button
            text="Questions de compréhension"
            @tap="typeRevision = 'questions'"
            :backgroundColor="typeRevision === 'questions' ? '#2563EB' : '#DBEAFE'"
            :color="typeRevision === 'questions' ? '#FFFFFF' : '#1E40AF'"
            marginBottom="5"
          />

          <Button
            text="Résumé + Questions"
            @tap="typeRevision = 'complet'"
            :backgroundColor="typeRevision === 'complet' ? '#2563EB' : '#DBEAFE'"
            :color="typeRevision === 'complet' ? '#FFFFFF' : '#1E40AF'"
            marginBottom="10"
          />

          <Label
            :text="'Mode sélectionné : ' + nomMode"
            fontSize="13"
            color="#64748B"
            marginBottom="15"
          />

          <!-- Génération -->
          <Button
            :text="carregando ? 'Génération en cours...' : 'Générer ma révision'"
            :isEnabled="!carregando"
            @tap="genererRevision"
            backgroundColor="#2563EB"
            color="#FFFFFF"
            fontSize="16"
            marginBottom="20"
          />

          <ActivityIndicator
            :busy="carregando"
            color="#2563EB"
            v-if="carregando"
            marginBottom="15"
          />

          <!-- Résultat -->
          <Label
            text="Résultat de l'IA"
            fontSize="18"
            fontWeight="bold"
            color="#334155"
            marginBottom="10"
          />

          <StackLayout
            backgroundColor="#FFFFFF"
            padding="15"
            marginBottom="15"
          >
            <Label
              :text="resultatFormate"
              textWrap="true"
              fontSize="15"
              color="#334155"
              lineHeight="5"
            />
          </StackLayout>

          <Label
            text="Les réponses de l'IA doivent être vérifiées."
            fontSize="12"
            color="#64748B"
            textWrap="true"
            marginBottom="20"
          />

        </StackLayout>
      </ScrollView>
    </Page>
  </Frame>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const texte = ref('')
const typeRevision = ref('resume')
const resultat = ref('Le résultat apparaîtra ici.')
const carregando = ref(false)

const nomMode = computed(() => {
  const noms: Record<string, string> = {
    resume: 'Résumé',
    questions: 'Questions de compréhension',
    complet: 'Résumé + Questions'
  }

  return noms[typeRevision.value] || 'Résumé'
})

// Nettoyage des marqueurs Markdown
const resultatFormate = computed(() => {
  return resultat.value
    .replace(/\*\*/g, '')
    .replace(/^#{1,6}\s+/gm, '')
    .replace(/\r\n/g, '\n')
    .trim()
})

async function genererRevision() {
  if (carregando.value) return

  if (!texte.value.trim()) {
    resultat.value = 'Veuillez saisir un texte à réviser.'
    return
  }

  if (texte.value.length > 10000) {
    resultat.value =
      'Le texte ne doit pas dépasser 10 000 caractères.'
    return
  }

  carregando.value = true
  resultat.value = 'Génération en cours...'

  try {
    const response = await fetch(
      'http://10.0.2.2:3000/revision',
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          texte: texte.value,
          typeRevision: typeRevision.value
        })
      }
    )

    const data = await response.json()

    if (!response.ok) {
      throw new Error(
        data.erreur || 'Erreur du serveur.'
      )
    }

    resultat.value =
      data.resultat || 'Aucun résultat généré.'

  } catch (error) {
    console.error('Erreur RévisIA :', error)

    if (error instanceof Error) {
      if (
        error.message.includes('Failed to fetch') ||
        error.message.includes('Network request failed')
      ) {
        resultat.value =
          'Impossible de communiquer avec le serveur IA. Vérifiez que le serveur Node.js et Ollama sont actifs.'
      } else {
        resultat.value = error.message
      }
    } else {
      resultat.value =
        'Une erreur inattendue est survenue.'
    }

  } finally {
    carregando.value = false
  }
}
</script>
