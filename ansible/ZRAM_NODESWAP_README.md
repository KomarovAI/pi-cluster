# ZRAM + K3s NodeSwap Configuration

## Что добавлено

### 1. ZRAM Setup Role (`roles/zram_setup`)
- Автоматически настраивает ZRAM на всех нодах
- **256MB на каждое ядро CPU** + размер оперативной памяти
- Алгоритм сжатия: LZ4 (по умолчанию)
- Создаёт systemd service для автозапуска

### 2. K3s NodeSwap Role (`roles/k3s_nodeswap`)
- Включает Kubernetes 1.30+ NodeSwap feature
- Конфигурирует kubelet с `fail-swap-on=false`
- Устанавливает `swapBehavior: LimitedSwap` (только Burstable/BestEffort поды)

## Использование

### Установка ZRAM (без K3s)
# ============================================================================
# НОВАЯ РОЛЬ: ansible/roles/zram_setup
# ============================================================================

# Создаём структуру роли
mkdir -p ansible/roles/zram_setup/{tasks,templates,defaults,handlers}

# ----------------------------------------------------------------------------
# ansible/roles/zram_setup/defaults/main.yml
# ----------------------------------------------------------------------------
cat > ansible/roles/zram_setup/defaults/main.yml << 'EOF'
---
# ZRAM configuration
zram_size_mb_per_core: 256
zram_compression_algorithm: lz4  # lz4, lzo, zstd
zram_num_devices: 1
